## Features

The scope_provider package provides a structured way to manage state and business logic in Flutter 
applications using the Bloc pattern. It introduces new types of instances and enforces certain 
rules to ensure consistency and reliability.  

New Types of Instances
- ScopeController: An abstract class that serves as a controller for a Bloc. It provides a way to 
create or retrieve a Bloc instance and offers methods to interact with the Bloc and perform actions 
based on the state.  
- ScopeProvider: An abstract class that acts as a base for any StatefulWidget that wants to provide 
a ScopeController, Bloc, and listen to state changes. It ensures that the widget is tightly coupled 
with the specific Bloc and controller it is managing.  

## Rules
**State and Event Classes**: Each state and event of a Bloc should extend BlocState and BlocEvent 
respectively!!!. 
This ensures that states can be compared by value rather than by reference, which is useful in the 
Bloc pattern. 

Generic Parameters: ScopeProvider requires four type parameters to function correctly:  
- SF: The type of the StatefulWidget that the ScopeProvider is extending.
- BS: The type of BlocState (the state managed by the Bloc).
- BL: The type of Bloc that handles BlocEvent and emits BlocState.
- SC: The type of ScopeController that creates or manages the Bloc.

## Description for scope_provider.dart
The scope_provider.dart file defines the core components of the scope_provider package:
- BlocState: An abstract class that all states of a Bloc must extend. It uses the Equatable package 
to ensure states can be compared by value rather than by reference.
- BlocEvent: An abstract class that all events of a Bloc must extend. Events in the Bloc pattern 
represent actions that trigger state changes.
- ScopeController: An abstract class that serves as a controller for a Bloc. It provides a way to 
create or retrieve a Bloc instance and offers methods to interact with the Bloc and perform actions 
based on the state.
- _ScopeBuilder: A StatelessWidget that is responsible for building the Bloc and using it with the 
provided ScopeController. It's an internal helper widget.
- ScopeProvider: An abstract class that acts as a base for any StatefulWidget that wants to provide 
a ScopeController, Bloc, and listen to state changes. It ensures that the widget is tightly coupled 
with the specific Bloc and controller it is managing.
- _ScopeInherited: A custom InheritedWidget that holds the ScopeController and the current 
BlocState. This widget allows its descendants to access the controller and the current state, and 
it decides whether to notify listeners when the state changes.


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
  /// ensure that event and state are extended from BlocEvent and BlocState
  MessageBloc get createBloc => MessageBloc();

  bool isCanShowMessage(BuildContext context) {
    /// A getBloc returns the bloc instance of MessageBloc because it is extended 
    /// from ScopeController that has a type of MessageBloc
    final bloc = getBloc(context);
    // Your logic here
  }

  String getMessage(BuildContext context) {
    final bloc = getBloc(context);
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

ScopeProvider is a generic class that requires four type parameters to function correctly:  
- MessageBuilder: This is the type of the StatefulWidget that the ScopeProvider is extending. 
It ensures that the ScopeProvider is tightly coupled with the specific widget it is managing.  
- MessageState: This represents the state managed by the Bloc. It is used to define the type of 
state that the Bloc will emit and the ScopeProvider will listen to.  
- MessageBloc: This is the Bloc that handles events and emits states. It encapsulates the business 
logic and state management for the ScopeProvider.  
- MessageController: This is the ScopeController that creates or manages the Bloc. It provides 
methods to interact with the Bloc and perform actions based on the state.

By specifying these types, ScopeProvider can correctly instantiate and manage the Bloc, listen to state changes, and provide the necessary context and controller to the widget tree.
