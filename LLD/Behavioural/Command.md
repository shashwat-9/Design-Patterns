### Command

 - Command is a behavioral design pattern that turns a request/operation into an object, thereby decoupling the object that
initiates the request from the object that performs it. Since requests become objects, they can be stored, queued, logged,
retried, replayed, composed into macros, or used to implement undo/redo.

```java
    public interface Command {
        void execute();
        void undo();
    }
    
    public class Light {
    
    }

    public class LightOnCommand implements Command{
    
    }

    public class LightOffCommand implements Command {
    
    }
    
    public class FanOffCommand implements Command {
    
    }
    
    public class RemoteControl {
        private Stack<Command> history;
        private Command command;
        
        void setCommand(Command command) {
            this.command = command;
            history.push(command);
        }
        
        void pressButton() {
            command.execute();
        }
        
        void undo() {
            if (!history.isEmpty()) {
                Command recentCommand = history.pop();
                recentCommand.undo();
            }
        }
    }
    
    /*
     * Command command = new FanOffCommand();
     * command.execute();
     * 
     * */
```

##### Components

1. Command interface → defines operation/request/state interface
2. Concrete implementations → Implements the command interface a/c to the behavior of the operation/request/state.
e.g. LightOnCommand, LightOffCommand.
3. Invoker → Class that triggers the command.execute() method
4. Receiver → The class that actually performs the operation. e.g. Light, Fan etc.
5. Client -> Creates/Configure commands

##### Undo Feature
 - In the Command interface we can add undo operation for the concrete command classes.
 - We can keep a Stack of Command in the invoker class which will be popped whenever undo is called in the invoker which
will call the undo of the popped Command object.

##### Properties
 - Command can carry Operations/Requests/State
 - Commands can be used in a queue of commands
```
    Queue<Command> queue = new LinkedList<>();
    queue.add(SendEmailCommand());
    queue.add(GenerateReportCommand());
    queue.add(PaymentProcessingCommand());
    
    while (!queue.isEmpty()) {
        Command command = queue.poll();
        command.execute();
    }
```
 - Macro command is a command implementing class containing multiple commands(list of commands).
```
    class MacroCommand implements Command {
        private final List<Command> commands;
        
        MacroCommands(List<Commands> commands) {
        
            this.commands = commands;
        }
        
        @Override
        public void execute() {
            for (Command command : commands) {
                command.execute();
            }
        }
    }
```
 - SRP and OCP are supported by implementing the command pattern.