# SODA 1.0.0
Simple Oracle Document Access (SODA) is a new family of APIs from Oracle that let you create Oracle Database applications at a level of simplicity and ease-of-use typically associated with NoSQL document stores. More specifically, SODA lets you create and store collections of documents in Oracle Database, retrieve them, and query them, without needing to know Structured Query Language (SQL) or how the data in the documents is stored in the database.

SODA for Java is a Java library that provides SODA. You can use it to perform create, retrieve, update, and delete (CRUD) operations on documents of any kind, and you can use it to query JSON documents.

With the SODA architecture, a database contains collections, and each collection contains documents. SODA is designed primarily for working with JSON documents, but a document can be of any Multipurpose Internet Mail Extensions (MIME) type.

**This is an open source project maintained by Oracle Corp.**

SODA is built on top of native JSON support in the Oracle database.

See the [Oracle as a Document Store](http://www.oracle.com/technetwork/database/application-development/oracle-document-store/index.html) page on the Oracle Technology Network for more info.

### Getting started

The following short code snippet illustrates working with SODA. It shows how to create a document collection, insert a document into it, and query the collection by using a unique document key and a QBE (query-by-example).

```java        
// Get an OracleRDBMSClient - starting point of SODA for Java application.
OracleRDBMSClient cl = new OracleRDBMSClient();

// Get a database.
OracleDatabase db = cl.getDatabase(conn);

// Create a collection with the name "MyJSONCollection".
OracleCollection col = db.admin().createCollection("MyJSONCollection");

// Create a JSON document.
OracleDocument doc =
  db.createDocumentFromString("{ \"name\" : \"Alexander\" }");

// Insert the document into a collection, and get back its
// auto-generated key.
String k = col.insertAndGet(doc).getKey();

// Find a document by its key. The following line
// fetches the inserted document from the collection
// by its unique key, and prints out the document's content
System.out.println ("Inserted content:" + 
                    col.find().key(k).getOne().getContentAsString());
                    
// Find all documents in the collection matching a query-by-example (QBE).
// The following lines find all JSON documents in the collection that have 
// a field "name" that starts with "A".
OracleDocument f = db.createDocumentFromString("{\"name\" : { \"$startsWith\" : \"A\" }}");
                       
OracleCursor c = col.find().filter(f).getCursor();

while (c.hasNext())
{
    // Get the next document.
    OracleDocument resultDoc = c.next();

    // Print the document key and content.
    System.out.println ("Key:         " + resultDoc.getKey());
    System.out.println ("Content:     " + resultDoc.getContentAsString());
}
```

Note that there's no SQL or JDBC programming required. Under the covers, SODA for Java transparently converts operations on document collections into SQL and executes it over JDBC.

See [Getting Started with SODA for Java](https://github.com/oracle/SODA-FOR-JAVA/blob/master/doc/Getting-started-example.md) for a complete introductory example.

### Documentation

The documentation is located [here](http://docs.oracle.com/cd/E63251_01/index.htm).

The Javadoc is located [here](http://oracle.github.io/SODA-FOR-JAVA).

### Build

SODA for Java source code is built with Ant and (optionally) Ivy. See [Building the source code](https://github.com/oracle/SODA-FOR-JAVA/blob/master/doc/Building-source-code.md) for
details. 

SODA for Java comes with a testsuite, built with JUnit and driven by Ant. See [Building and running the tests]
(https://github.com/oracle/SODA-FOR-JAVA/blob/master/doc/Building-and-running-tests.md) for details.

### Contributions

SODA is an open source project. See [Contributing](https://github.com/oracle/SODA-FOR-JAVA/blob/master/CONTRIBUTING.md) for details.

Oracle gratefully acknowledges the contributions to SODA made by the community.

### Getting in touch

Please open an issue [here](https://github.com/oracle/SODA-FOR-JAVA/issues), or post to the [ORDS, SODA, and JSON in the database forum] (https://community.oracle.com/community/database/developer-tools/oracle_rest_data_services/) with SODA-FOR-JAVA in the subject line.
