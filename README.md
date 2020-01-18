## sub·3期·第③组 BitDAO (⁎⁍̴̛ᴗ⁍̴̛⁎)

### 项目简介
  - 项目名称: 猫侠
  - 项目类型: DAPP游戏
  - 项目内容: 玩家通过打野或挑战的方式获取经验，不断提升猫侠级别，攻击力，通过购买装备增加攻击力
  - 项目参与人: 郝明，李士佳，朱强，徐贺辉
  - 名词
    + 经验值：EXP（Experience）
    + 战斗力：CE（Combat Effectiveness)
    + 血量：HP(Health Point）

### 操作流程

+ 初始化数据

装备类型：头盔、铠甲、武器、鞋子

区块链启动创世区块初始化8件装备,每类两件（疯狂面具,吸血面具,刃甲,强袭装甲,羊刀,圣剑，精灵皮靴,飞鞋）

可通过root权限添加新装备，设置装备类型、名称、攻击力、价格

```
pub fn add_weaponry(origin,kind:WeaponryKind,name:Vec<u8>,combat_effectiveness:u32,price:BalanceOf<T>)
```

通过root权限添加野怪，设置血量、攻击力

```
pub fn create_wild_animal(origin, health_point:u32, combat_effectiveness:u32) 
```
+ 创建小猫
  
小猫创建，默认攻击力100，经验0，血量100

```
pub fn create(origin) 
```

+ 购买武器 
  
通过购买武器增加小猫攻击力，每类装备只能佩戴一件，钱默认转给root

```
pub fn buy_weaponry(origin,kitty_id:T::KittyIndex,weaponry_id:WeaponryIndex)
```

+ 打野升级
  
打野可以升级经验，增加攻击力
选择自己的小猫与指定野怪，会受到伤害减少血量
```
pub fn battle_wild(origin,kitty_id:T::KittyIndex,wild_animal_id:u32)
```

+ 对战

选择其他玩家的小猫进行挑战，败者血量归零，选择的猫与擂主都需要处于空闲状态，对战后双方猫会冷却一定时间

```
pub fn battle(origin,kitty_id:T::KittyIndex,target_id:T::KittyIndex)
```

+ 加血

受到伤害后，在回合战之后可以支付token进行回血,token默认支付给root

```
pub fn full_health(origin,kitty_id:T::KittyIndex)
```

+ 抽奖
  
系统通过offchainworker实现定期抽奖功能，随机抽取一只小猫增加攻击力

```
let random_seed  = payload.using_encoded(blake2_128);
let now = <timestamp::Module<T>>::get();
let random_seed = BlakeTwo256::hash(&random_seed);
let mut rng = <RandomNumberGenerator<BlakeTwo256>>::new(random_seed);
let random = rng.pick_u32(kitty_count.into()-1);
```
+ 挂牌交易

```
pub fn ask(origin, kitty_id: T::KittyIndex, price: Option<BalanceOf<T>>) 
```
+ 购买
```
pub fn buy(origin, kitty_id: T::KittyIndex, price: BalanceOf<T>) 
```
+ 繁殖

```
fn do_breed(sender: &T::AccountId, kitty_id_1: T::KittyIndex, kitty_id_2: T::KittyIndex) -> result::Result<T::KittyIndex, DispatchError> 
```

### 数据结构

```json
{
  "Kitty": "[u8, 16]",
  "KittyIndex": "u32",
  "KittyLinkedItem": {
    "prev": "Option<KittyIndex>",
    "next": "Option<KittyIndex>"
  },
  "BalanceOf": "Balance",
  "Weaponry": {
    "name": "Vec<u8>",
    "kind": "WeaponryKind",
    "ce": "u32",
    "price": "BalanceOf"
  },
  "WeaponryIndex": "u32",
  "KittyAttr": {
    "hp": "u32",
    "exp": "u32",
    "ce": "u32",
    "battle_begin": "Option<Moment>",
    "battle_end": "Option<Moment>",
    "battle_type": "Option<BattleType>"
  },
  "WeaponryKind": {
    "_enum": [
      "HELMET",
      "ARMOR",
      "WEAPON",
      "SHOES"
    ]
  },
  "BattleType": {
    "_enum": [
      "WILD",
      "KITTY"
    ]
  },
  "WildAnimal": {
    "hp": "u32",
    "ce": "u32"
  }
}
```