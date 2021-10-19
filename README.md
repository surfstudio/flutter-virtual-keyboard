# Virtual Keyboard

[![Build Status](https://shields.io/github/workflow/status/surfstudio/SurfGear/build?logo=github&logoColor=white)](https://github.com/surfstudio/SurfGear/tree/main/packages/virtual_keyboard)

This package made by [Surf](https://surf.ru).

## Description

Keyboard widget for use in widget tree

## Installation

Add `virtual_keyboard` to your `pubspec.yaml` file:

```yaml
dependencies:
  virtual_keyboard: ^1.0.0
```

You can use both `stable` and `dev` versions of the package listed above in the badges bar.

## Example

```dart
static const _maxCount = 4;

var _symbols = '';

int get _symbolsCount => _symbols.length;

@override
Widget build(BuildContext context) {
  return Scaffold(
    appBar: AppBar(
      title: Text(widget.title),
    ),
    body: Center(
      child: Column(
        mainAxisSize: MainAxisSize.min,
        children: [
          Text(_symbols),
          const SizedBox(height: 20),
          Padding(
            padding: const EdgeInsets.all(50),
            child: VirtualKeyboardWidget(
              virtualKeyboardEffect: VirtualKeyboardEffect.keyRipple,
              keyboardKeys: numericKeyboardKeys,
              onPressKey: _handleTapKey,
            ),
          ),
        ],
      ),
    ),
  );
  }

VirtualKeyboardKey _buildClear() {
  return VirtualKeyboardDeleteKey(
    useAsKey: true,
    widget: InkWell(
      splashColor: Colors.green,
      onTap: () {
        _symbols = '';
        numericKeyboardKeys[3][2] = buildDelete();
        setState(() {});
      },
      child: const SizedBox(
        height: 50,
        child: Center(child: Text('Clear')),
      ),
    ),
  );
}

void _handleTapKey(VirtualKeyboardKey key) {
  if (key is VirtualKeyboardDeleteKey) {
    if (_symbolsCount == 0) {
      return;
    }

    _symbols = _symbols.substring(0, _symbolsCount - 1);
  } else if (key is VirtualKeyboardNumberKey) {
    _symbols += key.value;
  }

  if (_symbolsCount >= _maxCount) {
    numericKeyboardKeys[3][2] = _buildClear();
  } else {
    numericKeyboardKeys[3][2] = buildDelete();
  }

  setState(() {});
}
}

/// Клавиши для цифровой экранной клавиатуры
List<List<VirtualKeyboardKey>> numericKeyboardKeys = [
  for (var i = 1; i < 4; i++)
    [
      for (var j = 1; j < 4; j++) VirtualKeyboardNumberKey((i * j).toString()),
    ],
  [
    VirtualKeyboardEmptyStubKey(),
    VirtualKeyboardNumberKey(
      '0',
      widget: const Text('Zero'),
      keyDecoration: BoxDecoration(
        color: Colors.red.withOpacity(.1),
      ),
    ),
    buildDelete(),
  ],
];

VirtualKeyboardKey buildDelete() {
  return VirtualKeyboardDeleteKey(widget: const Text('delete'));
}
```

## Changelog

All notable changes to this project will be documented in [this file](./CHANGELOG.md).

## Issues

For issues, file directly in the Issues section.

## Contribute

If you would like to contribute to the package (e.g. by improving the documentation, solving a bug or adding a cool new feature), please review our [contribution guide](../../CONTRIBUTING.md) first and send us your pull request.

Your PRs are always welcome.

## How to reach us

Please feel free to ask any questions about this package. Join our community chat on Telegram. We speak English and Russian.

[![Telegram](https://img.shields.io/badge/chat-on%20Telegram-blue.svg)](https://t.me/SurfGear)

## License

[Apache License, Version 2.0](https://www.apache.org/licenses/LICENSE-2.0)
