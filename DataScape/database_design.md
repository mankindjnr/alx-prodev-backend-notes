# Database Design: Entity Relationship Diagrams, Normalization, and Schema Design
***
Undertanding these concepts is essential for creating a well-structured efficient, and scalable database.

1. ## Entity-Relationship (ER) Diagrams
- __Definition__ ER Diagrams are visual representations of the entities (tables) in a database and their relationships. They help in understanding how data is organized and how different entities interact with each other.

- __Components__
    - __Entities__: Represent objects or concepts, e.g Users, payment, Booking
    - __Attributes__: Properties or details of entities, e.g User ID, Payment Date, Price
    - __Relationships__: Connections between entitities e.g , a user makes a Booking, and   
            a booking includes a payment. 

- __How to Create__:
    1. Identify Entities: Determine the main objects that need to be represented.
    2. Define Attributes: List the attributes for each entity
    3. Establish Relationships: Define how entities are related to each other.
    4. Draw the diagram: Use diagramming tools to visualize entities, attributes, and relationships.

ER Diagrams provide a clear overview of the database structure, facilitate communication between stakeholders, and help in designing a coherent schema.

2. ## Normalization
- __Definition__: Normalization is the process of organizing data to reduce redundancy and improve data integrity. It involves dividing a database into two or more tables and defining relationships between them.

- __Normalization Forms__
    - __First Normal Form (1NF)__: Ensure each column contains atomic values, and each record is unique.
    - __Second Normal Form (2NF)__: Achieve 1NF and ensure all non-key attributes are fully functionally dependent on the primary key.
    - __ThirdNormal Formal (3NF)__: Achieve 2NF and ensure all attributes are functionally dependent only on the primary key.

### How to  Normalize
1. __Identify Functional Dependencies__: Determine which attributes are dependent on others.
2. __Decompose Tables__: Break tables into smaller tables to remove redundancy.
3. __Define Relationships__: Use foreign Keys to maintain relationships between decomposed tables.
- __Importance__: Normalization reduces data redundancy, prevents anomalies, and ensures data consisitency, it simplifies data maintenance and imporoves database performance.

3. ## Schema Design
- __Definition__: Schema design involves creating the database schema which defines the structure
of tables, columns, data types, and constraints. It servers as the blueprint for the database.

- __Components__
    - __Tables__: Define the structture of data storage.
    - __Columns__: Specify data types and constraints for each table.
    - __Primary Keys__: Uniquely identify each record in a table.
    - __Foreign Keys__: Maintain relationships between tables.
    - __Constraints__: Define rules of dta integrity (e.g NOT NULL, UNIQUE)

- __How to Design a Schema__
    1. __Define requirements__: Understand the data requirements and relationships.
    2. __Create Tables__: Design tables based on entities and attributes.
    3. __Set Data Types__: Choose appropriate data type for each column
    4. __Establish Keys__: Define primary and foreign keys to enforce relationships.
    5. __Apply Constraints__: Implement constraints to ensure data integrity.
- __Importance__: A well-designed schema ensures efficient data storage, retrieval. and integrity. It provides a foundation for building and scaling applications effectively.


## Database Specification - AirBnB
1. ### Entities and Attributes
- User
    user_id: Primary Key, UUID, Indexed
    first_name: VARCHAR, NOT NULL
    last_name: VARCHAR, NOT NULL
    email: VARCHAR, UNIQUE, NOT NULL
    password_hash: VARCHAR, NOT NULL
    phone_number: VARCHAR, NULL
    role: ENUM (guest, host, admin), NOT NULL
    created_at: TIMESTAMP, DEFAULT CURRENT_TIMESTAMP
- Property
    property_id: Primary Key, UUID, Indexed
    host_id: Foreign Key, references User(user_id)
    name: VARCHAR, NOT NULL
    description: TEXT, NOT NULL
    location: VARCHAR, NOT NULL
    pricepernight: DECIMAL, NOT NULL
    created_at: TIMESTAMP, DEFAULT CURRENT_TIMESTAMP
    updated_at: TIMESTAMP, ON UPDATE CURRENT_TIMESTAMP
- Booking
    booking_id: Primary Key, UUID, Indexed
    property_id: Foreign Key, references Property(property_id)
    user_id: Foreign Key, references User(user_id)
    start_date: DATE, NOT NULL
    end_date: DATE, NOT NULL
    total_price: DECIMAL, NOT NULL
    status: ENUM (pending, confirmed, canceled), NOT NULL
    created_at: TIMESTAMP, DEFAULT CURRENT_TIMESTAMP
- Payment
    payment_id: Primary Key, UUID, Indexed
    booking_id: Foreign Key, references Booking(booking_id)
    amount: DECIMAL, NOT NULL
    payment_date: TIMESTAMP, DEFAULT CURRENT_TIMESTAMP
    payment_method: ENUM (credit_card, paypal, stripe), NOT NULL
- Review
    review_id: Primary Key, UUID, Indexed
    property_id: Foreign Key, references Property(property_id)
    user_id: Foreign Key, references User(user_id)
    rating: INTEGER, CHECK: rating >= 1 AND rating <= 5, NOT NULL
    comment: TEXT, NOT NULL
    created_at: TIMESTAMP, DEFAULT CURRENT_TIMESTAMP
- Message
    message_id: Primary Key, UUID, Indexed
    sender_id: Foreign Key, references User(user_id)
    recipient_id: Foreign Key, references User(user_id)
    message_body: TEXT, NOT NULL
    sent_at: TIMESTAMP, DEFAULT CURRENT_TIMESTAMP

2. ### Constraints
- User Table
    Unique constraint on email.
    Non-null constraints on required fields.
    Property Table
    Foreign key constraint on host_id.
    Non-null constraints on essential attributes.
- Booking Table
    Foreign key constraints on property_id and user_id.
    status must be one of pending, confirmed, or canceled.
- Payment Table
    Foreign key constraint on booking_id, ensuring payment is linked to valid bookings.
- Review Table
    Constraints on rating values (1-5).
    Foreign key constraints on property_id and user_id.
- Message Table
    Foreign key constraints on sender_id and recipient_id.
- Indexing
    Primary Keys: Indexed automatically.
    Additional Indexes:
        email in the User table.
        property_id in the Property and Booking tables.
        booking_id in the Booking and Payment tables.