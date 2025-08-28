<h1 align="center">mineflayer-auto-eat</h1>

> A customizable and flexible auto-eat utility plugin for Mineflayer bots

## Table of Contents

-   [Table of Contents](#table-of-contents)
-   [Install](#install)
-   [Example](#example)
-   [API](#api)
    -   [Properties](#properties)
        -   [bot.autoEat.enabled](#botautoeatenabled)
        -   [bot.autoEat.isEating](#botautoeatiseating)
        -   [bot.autoEat.opts](#botautoeatopts)
        -   [bot.autoEat.foods](#botautoeatfoods)
        -   [bot.autoEat.foodsArray](#botautoeatfoodsarray)
        -   [bot.autoEat.foodsByName](#botautoeatfoodsbyname)
    -   [Methods](#methods)
        -   [bot.autoEat.setOpts(opts: Partial\<IEatUtilOpts\>)](#botautoeatsetoptsopts-partialieatutilopts)
        -   [bot.autoEat.eat(opts: EatOptions)](#botautoeateatopts-eatoptions)
        -   [bot.autoEat.enableAuto()](#botautoeatenableauto)
        -   [bot.autoEat.disableAuto()](#botautoeatdisableauto)
        -   [bot.autoEat.cancelEat()](#botautoeatcanceleat)
    -   [Settings](#settings)
        -   [IEatUtilOpts](#ieatutilopts)
        -   [EatOpts](#eatopts)
    -   [Events](#events)
-   [Authors](#authors)
-   [Show your support](#show-your-support)

## Install

```sh
npm install mineflayer-auto-eat
```

## Basic Usage

```javascript
const mineflayer = require('mineflayer')
const autoEat = require('mineflayer-auto-eat')

const bot = mineflayer.createBot({
  host: 'localhost',
  username: 'Player',
})

// Load the plugin
bot.loadPlugin(autoEat)

// The plugin is enabled by default
// You can disable and re-enable as needed
bot.autoEat.disableAuto()
bot.autoEat.enableAuto()

// Listen to eating events
bot.on('autoeat_started', (item, offhand) => {
  console.log(`Started eating ${item.name} (offhand: ${offhand})`)
})

bot.on('autoeat_finished', (item, offhand) => {
  console.log(`Finished eating ${item.name}`)
})

bot.on('autoeat_error', (error) => {
  console.error(`Error while eating: ${error.message}`)
})

// Configure the plugin
bot.autoEat.setOpts({
  priority: 'foodPoints',  // 'foodPoints', 'saturation', 'effectiveQuality', 'saturationRatio', or 'auto'
  minHunger: 14,           // Eat when hunger reaches this level
  minHealth: 16,           // Use saturation-focused foods when health is below this level
  bannedFood: ['rotten_flesh', 'poisonous_potato']  // Foods to never eat
})

// Manually trigger eating
bot.autoEat.eat()
  .then(() => console.log('Finished eating'))
  .catch(err => console.error('Failed to eat:', err))
```

## Configuration Options

You can configure the plugin using `bot.autoEat.setOpts(options)` with these options:

| Option | Type | Default | Description |
|--------|------|---------|-------------|
| `priority` | string | `'auto'` | Food selection priority strategy ('foodPoints', 'saturation', 'effectiveQuality', 'saturationRatio', or 'auto') |
| `minHunger` | number | `15` | Minimum hunger level before eating |
| `minHealth` | number | `14` | Health threshold for prioritizing food with high saturation |
| `bannedFood` | string[] | `[...]` | Array of food item names to never eat |
| `returnToLastItem` | boolean | `true` | Re-equip previous item after eating |
| `offhand` | boolean | `false` | Use offhand for eating |
| `eatingTimeout` | number | `3000` | Milliseconds to wait before timing out an eating attempt |
| `strictErrors` | boolean | `true` | Whether to throw errors or just log them |
| `checkOnItemPickup` | boolean | `true` | Check if food should be eaten when picking up items |
| `chatNotifications` | boolean | `true` | Send chat messages when out of food |

## Advanced Usage

### Manual Eating with Options

```javascript
bot.autoEat.eat({
  food: 'cooked_beef',    // Item name, Item object, or foodId
  offhand: false,         // Whether to use offhand
  equipOldItem: true,     // Return to previous item after eating
  priority: 'saturation'  // Override default priority for this eat operation
})
```

### Customizing Events

```javascript
// Using the new EventEmitter API
bot.autoEat.on('eatStart', (opts) => {
  console.log(`Starting to eat ${opts.food.name}`)
})

bot.autoEat.on('eatFinish', (opts) => {
  console.log(`Finished eating ${opts.food.name}`)
})

bot.autoEat.on('eatFail', (error) => {
  console.error(`Eating failed: ${error.message}`)
})
```

## Authors

👤 **Rocco A**

https://github.com/GenerelSchwerz

👤 **Linkle**

https://github.com/linkle69

## Show your support

Give a ⭐️ if this plugin helped you!
