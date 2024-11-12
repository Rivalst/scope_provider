<!--
This README describes the package. If you publish this package to pub.dev,
this README's contents appear on the landing page for your package.

For information about how to write a good package README, see the guide for
[writing package pages](https://dart.dev/tools/pub/writing-package-pages).

For general information about developing packages, see the Dart guide for
[creating packages](https://dart.dev/guides/libraries/create-packages)
and the Flutter guide for
[developing packages and plugins](https://flutter.dev/to/develop-packages).
-->

TODO: Put a short description of the package here that helps potential users
know whether this package might be useful for them.

## Features

TODO: List what your package can do. Maybe include images, gifs, or videos.

## Getting started

TODO: List prerequisites and provide or point to information on how to
start using the package.

## Usage

To use the scope_provider package, follow these steps:

```yaml
dependencies:
  scope_provider:
    path: ../scope_provider
```

Create a ScopeController for your Bloc:

```dart
class MessageController extends ScopeController<MessageBloc> {
  @override
  MessageBloc get createBloc => MessageBloc();

  bool isCanShowMessage(BuildContext context) {
    // Your logic here
  }

  String getMessage(BuildContext context) {
    // Your logic here
  }
}
```

Extend ScopeProvider in your StatefulWidget instead of basic State:
```dart
class MessageBuilder extends StatefulWidget {
  const MessageBuilder({super.key});
  

  @override
  State<MessageBuilder> createState() => _MessageBuilderState();
}

class _MessageBuilderState extends ScopeProvider<MessageBuilder, MessageState,
    MessageBloc, MessageController> {
  @override
  MessageController createController() {
    return MessageController();
  }

  @override
  Widget onBuild(BuildContext context) {
    return const SomeAnotherWidget();
  }

  @override
  void onListen(
      BuildContext context,
      MessageState state,
      MessageController controller,
      ) {
    final isCanShowMessage = controller.isCanShowMessage(context);
    if (isCanShowMessage) {
      DefaultSnackBar.show(
        context: context,
        message: controller.getMessage(context),
      );
    }
  }
}
```
Use the scope controller in your widget:

````dart
final messageController = ScopeProvider.of<MessageController>(context);
final message = messageController.getMessage(context);
````

## Additional information

TODO: Tell users more about the package: where to find more information, how to
contribute to the package, how to file issues, what response they can expect
from the package authors, and more.
