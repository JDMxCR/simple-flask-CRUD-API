# simple-flask-CRUD-API

## Overview

This Contact Management API allows users to perform Create, Read, Update, and Delete (CRUD) operations on contact information stored in a database. It is built using Python and Flask and uses a SQLite database for storage.

## Features

- **List Contacts**: Retrieve a list of all contacts.
- **Create Contact**: Add a new contact to the database.
- **Update Contact**: Modify an existing contact's details.
- **Delete Contact**: Remove a contact from the database.

```python
from flask import request, jsonify
from config import app, db
from models import Contact
```

- **Flask:** A web framework for Python, used to build web applications.

- **request:** A Flask object that contains data from the incoming HTTP request.

- **jsonify:** A Flask function that turns Python dictionaries into JSON responses.

- **config:** Presumably a Python module containing the configuration of the Flask app and database (db).

- **models:** A module where the Contact model is defined, likely representing the structure of a contact in the database.

```Python
@app.route("/contacts", methods=["GET"])
def get_contacts():
    contacts = Contact.query.all()
    json_contacts = list(map(lambda x: x.to_json(), contacts))
    return jsonify({"contacts": json_contacts}), 200
```

- **@app.route:** A decorator that tells Flask what URL should trigger the function below it.
- **"/contacts":** The endpoint URL for listing all contacts.
- **methods=["GET"]:** Specifies that this route responds to HTTP GET requests.
- **get_contacts():** The view function that is called when the /contacts endpoint is accessed.
- **Contact.query.all():** Retrieves all contact records from the database.
- **x.to_json():** Converts each contact record into JSON format.
- **jsonify(...):** Turns the list of contacts into a JSON response with a status code 200 (OK).

```Python
@app.route("/create_contact", methods=["POST"])
def create_contact():
    ...
    return jsonify({"message": "Contact created successfully"}), 201
```

- **methods=["POST"]:** Indicates that this route responds to HTTP POST requests, used for creating new resources.
- **create_contact():** The function that handles creating a new contact.
- **request.json.get(...):** Extracts data from the JSON payload of the request.
- **new_contact = Contact(...):** Creates a new Contact instance with the provided data.
- **db.session.add(new_contact):** Adds the new contact to the database session.
- **db.session.commit():** Commits the transaction, saving the new contact to the database.
- **201:** HTTP status code for “Created”.

```Python
@app.route("/update_contact/<int:contact_id>", methods=["PATCH"])
def update_contact(contact_id):
    ...
    return jsonify({"message": "Contact updated successfully"}), 200
```

- **methods=["PATCH"]:** Used for partial updates to a resource.
- **<int:contact_id >:** A variable part of the URL, capturing the contact’s ID as an integer.
- **update_contact(contact_id):** The function that updates an existing contact.
- **Contact.query.get(contact_id):** Fetches a single contact by its ID.
- **db.session.commit():** Saves the changes to the database.

```Python
@app.route("/delete_contact/<int:contact_id>", methods=["DELETE"])
def delete_contact(contact_id):
    ...
    return jsonify({"message": "Contact deleted successfully"}), 200
```

- **methods=["DELETE"]:** Indicates that this route responds to HTTP DELETE requests, used for deleting resources.
- **delete_contact(contact_id):** The function that deletes a contact.
- **db.session.delete(contact):** Marks the contact for deletion in the database session.
- **db.session.commit():** Removes the contact from the database.

```Python
if __name__ == "__main__":
    with app.app_context():
        db.create_all()
    app.run(debug=True)
```

- **if **name** == "**main**":** Checks if the script is being run directly (not imported).
- **app.app_context():** Creates a context for the Flask application to run within.
- **db.create_all():** Creates the database tables based on the models defined.
- **app.run(debug=True):** Starts the Flask application with debug mode enabled, which provides detailed error messages and live reloading.

This code snippet sets up a simple web server that can handle requests to manage contact information through a **RESTful API**. Each function corresponds to one of the **CRUD** operations, allowing users to interact with the contact data stored in a database. The use of **HTTP** methods **(GET, POST, PATCH, DELETE)** follows the **REST** architectural style, making it a **RESTful API**.
