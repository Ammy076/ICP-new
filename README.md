# ICP-new


Setting Up the Development Environment
Before developing your Canister Smart Contract, we need to set up our development environment. Make sure you have the latest version of the DFINITY Canister SDK installed. The SDK provides a command-line interface (CLI) tool called dfx for managing and deploying Canisters. You can verify the installation by running the following command in your terminal:

   dfx --version
If everything is installed correctly, you should see the version number displayed 1.

Creating the Message Board Canister
Now that our development environment is set up, let's create our Message Board Canister Smart Contract. Create a new directory for your project and navigate into it using the terminal. Run the following command to create a new project using the dfx project template:

   dfx new message_board
This will create a new project directory with the name message_board. Within the project directory, you'll find a few files and directories. The most important file is src/message_board.mo, which will contain the Motoko code for your Canister 1.

Writing the Message Board Canister Smart Contract
In the message_board.mo file, let's write our Message Board Canister Smart Contract. The Canister will be able to add, update, delete, and retrieve messages. For simplicity, each message will have a unique identifier and a text content.

   import Canister
   actor MessageBoard {
     var messages : [(Text, Text)] = [];

     public shared({}) func addMessage(id: Text, content: Text) : async () {
       messages := (id, content) :: messages;
     }

     public shared({}) func updateMessage(id: Text, newContent: Text) : async () {
       let newMessages = List.map(
         func (msg: (Text, Text)) : (Text, Text) {
           if (msg.0 == id) {
             (id, newContent)
           } else {
             msg
           }
         },
         messages
       );
       messages := newMessages;
     }

     public shared({}) func deleteMessage(id: Text) : async () {
       let newMessages = List.filter(
         func (msg: (Text, Text)) : Bool {
           msg.0 != id
         },
         messages
       );
       messages := newMessages;
     }

     public shared({}) func getMessage(id: Text) : async ?Text {
       for (msg in messages) {
         if (msg.0 == id) {
           return ?msg.1;
         };
       };
       return null;
     }

     public shared({}) func getMessages() : async [(Text, Text)] {
       messages;
     }
   }
This Canister Smart Contract defines an actor MessageBoard that has a list of messages and several public functions to manipulate and retrieve the messages 1.

Compiling and Deploying Your Canister
Now that our Canister Smart Contract is written, we need to compile and deploy it to the ICP network. In the terminal, navigate to your project directory and run the following commands:

   dfx canister create --all
   dfx canister install message_board
The first command will compile your Motoko code and create a canister for it on the ICP network. The second command will deploy your Canister to the ICP network 1.

Interacting with Your Canister
Now that our Canister is deployed, we can interact with it. We can call the addMessage, updateMessage, deleteMessage, getMessage, and getMessages functions defined in our Canister Smart Contract using commands like:

   dfx canister call message_board addMessage '("message1", "Hello, Internet Computer!")'
   dfx canister call message_board getMessage '("message1")'
   dfx canister call message_board getMessages
   dfx canister call message_board updateMessage '("message1", "Hello, World!")'
   dfx canister call message_board deleteMessage '("message1")'
Replace message_board with your Canister name. These commands will invoke the respective functions and show their results 1.
