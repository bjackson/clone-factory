# clone-factory
Simple clone contract factory


Check out the contracts/CloneFactory.sol file for details.  This code (intended to be called from an implementor factory contract)
will allow you to install a master copy of a contract, then easily (cheaply) create clones with separate state.  The deployed bytecode
just delegates all calls to the master contract address.

`npm install yarrumretep/clone-factory`

Then you can write code like this:
```
import "./Thing.sol";
import "clone-factory/contracts/CloneFactory.sol";
import "zeppelin-solidity/contracts/ownership/Ownable.sol";


contract ThingFactory is Ownable, CloneFactory {

  address public libraryAddress;

  event ThingCreated(address newThingAddress);

  function ThingFactory(address _libraryAddress) public {
    libraryAddress = _libraryAddress;
  }

  function setLibraryAddress(address _libraryAddress) public onlyOwner {
    libraryAddress = _libraryAddress;
  }

  function createThing(string _name, uint _value) public onlyOwner {
    address clone = createClone(libraryAddress);
    Thing(clone).init(_name, _value);
    ThingCreated(clone);
  }
}
```

This will inexpensively create a mimimalist forwarding shim contract that will delegate all calls to the contract libraryAddress
