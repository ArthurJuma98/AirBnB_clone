 0. README, AUTHORS
README.md contains a description of the project and the command interpreter (including how to start it, use it and examples).
AUTHORS contains a listing of individuals that have contributed content to the repository.
 1. Be PEP8 compliant!
All the Python files should pass PEP8 checks and the Bash scripts should pass the Shellcheck checks.
 2. Unittests
tests/ contains unit tests for all the classes and methods which work in both the interactive and the non-interactive modes.
 3. BaseModel
models/base_model.py contains a class BaseModel that defines all common attributes/methods for other classes:
Public instance attribute id (str) that is assigned with an uuid when an instance is created.
Public instance attribute created_at (datetime) that is assigned with the current datetime when an instance is created.
Public instance attribute updated_at (datetime) that is assigned with the current datetime when an instance is created and will be updated every time it is changed.
__str__ should return [<class name>] (<self.id>) <self.__dict__>.
Public instance method save(self) that updates the public instance attribute updated_at with the current datetime.
Public instance method to_dict(self) that updates the public instance attribute updated_at with the current datetime.
A key __class__ must be added to this dictionary with the class name of the object.
created_at and updated_at must be converted to string object in ISO format. Format: %Y-%m-%dT%H:%M:%S.%f (ex: 2017-06-14T22:31:03.285259).
 4. Create BaseModel from dictionary
models/base_model.py contains the following updates to its BaseModel class:
Contains the constructor __init__(self, *args, **kwargs) but *args won’t be used.
If kwargs is not empty:
Each key of this dictionary is an attribute name except __class__.
Each value of this dictionary is the value of this attribute name.
NOTE: created_at and updated_at are strings in this dictionary, but the BaseModel instance works with a datetime object. Thus these strings have to be converted into datetime objects from the string format %Y-%m-%dT%H:%M:%S.%f.
Otherwise, id and created_at are created as they were previously done.
 5. Store first object
models/engine/file_storage.py contains a class FileStorage that serializes instances to a JSON file and deserializes a JSON file to instances:
FileStorage contains the private class attributes:
__file_path: a string containing the path to the JSON file.
__objects: a dictionary that stores all objects by <class name>.id (e.g.; to store a BaseModel object with id=12121212, the key will be BaseModel.12121212).
FileStorage contains the public instance methods:
all(self): returns the dictionary __objects.
new(self, obj): sets in __objects the obj with key <obj class name>.id.
save(self): serializes __objects to the JSON file (path: __file_path).
reload(self): deserializes the JSON file to __objects only if the JSON file (__file_path) exists, otherwise nothing (including raising exceptions) is done.
models/init.py contains a module that creates a unique FileStorage instance for the application by creating the variable storage, an instance of FileStorage and calling the reload() method on this variable.
models/base_model.py contains the following updates:
Imports the storage variable from models/init.py.
Calls the save(self) method of the storage variable in the method save(self) of the BaseModel class.
In the __init__(self, *args, **kwargs) method of the BaseModel class, a call to the method new(self) is made on the storage variable if it’s (the BaseModel) a new instance (not from a dictionary representation).
 6. Console 0.0.1
console.py contains a program that contains the entry point of the command interpreter:
The cmd module must be used.
The class definition must be: class HBNBCommand(cmd.Cmd):.
The command interpreter should implement:
quit and EOF to exit the program.
help (this action is provided by default by cmd but it should be kept updated and documented as the tasks are worked on).
A custom prompt: (hbnb).
An empty line + ENTER shouldn’t execute anything.
The code should not be executed when imported.
 7. Console 0.1
The command interpreter (console.py) should have these commands:
create: Creates a new instance of BaseModel, saves it (to the JSON file) and prints the id.
Usage: create <class name>.
If the class name is missing, print ** class name missing **.
If the class name doesn’t exist, print ** class doesn't exist **.
show: Prints the string representation of an instance based on the class name and id.
Usage: show <class name> <id>.
If the class name is missing, print ** class name missing **.
If the class name doesn’t exist, print ** class doesn't exist **.
If the id is missing, print ** instance id missing **.
If the instance of the class name doesn’t exist for the id, print ** no instance found **.
destroy: Deletes an instance based on the class name and id (save the change into the JSON file).
Usage: destroy <class name> <id>.
If the class name is missing, print ** class name missing **.
If the class name doesn’t exist, print ** class doesn't exist **.
If the id is missing, print ** instance id missing **.
If the instance of the class name doesn’t exist for the id, print ** no instance found **.
all: Prints all string representation of all instances based or not on the class name.
Usage: all [<class name>].
The printed result must be a list of strings.
If the class name doesn’t exist, print ** class doesn't exist **.
update: Updates an instance based on the class name and id by adding or updating attribute (save the change into the JSON file).
Usage: update <class name> <id> <attribute name> "<attribute value>".
Only one attribute can be updated at a time.
It can be assumed that the attribute name is valid.
The attribute value must be casted to the attribute type.
If the class name is missing, print ** class name missing **.
If the class name doesn’t exist, print ** class doesn't exist **.
If the id is missing, print ** instance id missing **.
If the instance of the class name doesn’t exist for the id, print ** no instance found **.
If the attribute name is missing, print ** attribute name missing **.
If the value for the attribute name doesn’t exist, print ** value missing **.
All other arguments should not be used.
id, created_at and updated_at cant’ be updated. It can be assumed that they won’t be passed in the update command.
Only “simple” arguments can be updated: string, integer, and float. You can assume that nobody will try to update list of ids, dictionary or datetime.
Some rules for the command interpreter:
It can be assumed that arguments are always in the right order.
Each arguments are separated by a space.
A string argument with a space must be between double quote.
The error management starts from the first argument to the last one.
 8. First User
models/user.py contains a class User that inherits from BaseModel and contains the public class attributes: email (a string), password (a string), first_name (a string), and last_name (a string).
The FileStorage managed by models/engile/file_storage.py correctly serializes and deserializes the User class.
The command interpreter (console.py) should be able to allow the show, create, destroy, update, and all actions with the User class.
 9. More classes!
models/state.py contains a State class that inherits from BaseModel with the public class attribute name (a string).
models/city.py contains a City class that inherits from BaseModel with the public class attributes state_id (a string) and name (a string).
models/amenity.py contains an Amenity class that inherits from BaseModel with the public class attribute name (a string).
models/place.py contains a Place class that inherits from BaseModel with the public class attributes city_id (a string), user_id (a string), name (a string), description (a string), number_rooms (an integer), number_bathrooms (an integer), max_guest (an integer), price_by_night (an integer), latitude (a float), longitude (a float), and amenity_ids (a list of strings).
models/review.py contains a Review class that inherits from BaseModel with the public class attributes place_id (a string), user_id (a string), and text (a string).
 10. Console 1.0
The FileStorage managed by models/engile/file_storage.py correctly serializes and deserializes all the new classes: Place, State, City, Amenity, and Review.
The command interpreter (console.py) should be able to allow the show, create, destroy, update, and all actions with all the classes created previously.
 11. All instances by class name
The command interpreter (console.py) should be able to retrieve all instances of a class by using: <class name>.all().
 12. Count instances
The command interpreter (console.py) should be able to retrieve the number of instances of a class by using: <class name>.count().
 13. Show
The command interpreter (console.py) should be able to retrieve an instance based on its ID by using: <class name>.show(<id>).
 14. Destroy
The command interpreter (console.py) should be able to destroy an instance based on its ID by using: <class name>.destroy(<id>).
 15. Update
The command interpreter (console.py) should be able to update an instance based on its ID by using: <class name>.update(<id>, <attribute name>, <attribute value>).
 16. Update from dictionary
The command interpreter (console.py) should be able to update an instance based on its ID with a dictionary by using: <class name>.update(<id>, <dictionary representation>).
 17. Unittests for the Console!
tests/test_console.py contains unittests for console.py and all its features.
