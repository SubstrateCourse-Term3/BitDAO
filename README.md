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
    + ~~魔力值：MP(Magic Point)~~

# Substrate Node Template

A new SRML-based Substrate node, ready for hacking.

## Build

Install Rust:

```bash
curl https://sh.rustup.rs -sSf | sh
```

Initialize your Wasm Build environment:

```bash
./scripts/init.sh
```

Build Wasm and native code:

```bash
cargo build --release
```

## Run

### Single node development chain

Purge any existing developer chain state:

```bash
./target/release/substrate-kitties purge-chain --dev
```

Start a development chain with:

```bash
./target/release/substrate-kitties --dev
```

Detailed logs may be shown by running the node with the following environment variables set: `RUST_LOG=debug RUST_BACKTRACE=1 cargo run -- --dev`.

### Multi-node local testnet

If you want to see the multi-node consensus algorithm in action locally, then you can create a local testnet with two validator nodes for Alice and Bob, who are the initial authorities of the genesis chain that have been endowed with testnet units.

Optionally, give each node a name and expose them so they are listed on the Polkadot [telemetry site](https://telemetry.polkadot.io/#/Local%20Testnet).

You'll need two terminal windows open.

We'll start Alice's substrate node first on default TCP port 30333 with her chain database stored locally at `/tmp/alice`. The bootnode ID of her node is `QmRpheLN4JWdAnY7HGJfWFNbfkQCb6tFf4vvA6hgjMZKrR`, which is generated from the `--node-key` value that we specify below:

# Settings

1) Open [Polkadot UI](https://polkadot.js.org/apps/#/explorer) , Settings -> Local Node

2) Go to *Settings*, open *Developer* tab. Insert in textbox description of types (copy&paste from here) and Save it.

```json
{
  "Kitty": "[u8, 16]",
  "KittyIndex": "u32",
  "KittyLinkedItem": {
    "prev": "Option<KittyIndex>",
    "next": "Option<KittyIndex>"
  },
  "BalanceOf":"Balance",
  "Weaponry":{
  "name":"Vec<u8>",
  "kind":"WeaponryKind",
  "ce":"u32",
  "price":"BalanceOf"
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
  }
}
```