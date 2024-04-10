# Welcome to the Anythink Market repo (powered by [Wilco](https://www.trywilco.com))

To start the app1 use Docker. It will start both frontend and backend, including all the relevant dependencies, and the db.

Please find more info about each part in the relevant Readme file ([frontend](frontend/readme.md) and [backend](backend/README.md)).

## Development

When implementing a new feature or fixing a bug, please create a new pull request against `main` from a feature/bug branch and add `@vanessa-cooper` as reviewer.

## How to run in dev mode?

### Using Codespace
1.  run `docker compose up`
### On your local machine
1. [Install Docker](https://docs.docker.com/get-docker/)
2. [Install Docker Compose](https://docs.docker.com/compose/install/)
3. Run `docker compose up`.
4. 
<br>

### Playbook

* To empower our AI assistant, we've loaded movies data for you. Let's delve into understanding embeddings and vectors.
* Various tools like `Atlas Web UI`, `MongoDB Compass`, `MongoDB Shell`, or alternate MongoDB clients can be used to explore the data. For now, we'll focus on the Atlas Web UI.
* Navigate to `Deployment` > `Database` > `Browse Collections` in the Atlas Web UI.
* Within the `movies_quest` database, explore the `embedded_content` collection.
<br>

* Each document now contains a field named `embedding` a result of applying the `text-embedding-ada-002` model to the `plot` field.
* Think of the embedding model as a function; it takes text as input and returns a vector—a numerical representation in a multi-dimensional space. This representation captures semantic relationships, enabling tasks like sentiment analysis.
* Vectors have fixed dimensions, reflecting the number of features. For instance, a 3-dimensional vector has 3 elements.
* In the database, vectors are represented as arrays of numbers, where the array size corresponds to the dimensionality of the vector.
* Can you determine the number of elements (known as dimensionality of the vector) in the embedding field in the database? `1536`
<br>

* For our AI assistant to work, we need to create an index on the embedding field. Let's use the `Data Explorer` in the Atlas Web UI to create the index.
* Navigate to your cluster if you're not already there, click on `Atlas Search tab`, then `Create Search Index`, then under `Atlas Vector Search` choose the `JSON Editor`. Make sure you choose Vector Search index and not any other type
* In the Database and Collection section, choose the `movies_quest` database and the `embedded_content` database.
* In the dialog, enter the `embedding` field, its dimensions, and the similarity function
* You can keep the default index name as `vector_index`. Here is the complete JSON config for the index

*     {
      "fields": [
        {
          "numDimensions": 1536,
          "path": "embedding",
          "similarity": "cosine",
          "type": "vector"
        }
       ]
      }
<br>

* Open the `Codespace Web Environment` --> VSCode and run the terminal
* Add the `.env` file in the `backend` folder
*     VECTOR_SEARCH_INDEX_NAME=vector_index
      MONGODB_DATABASE_NAME=movies_quest
      MONGODB_CONNECTION_URI=your-connection-string
* Run the following in the terminal
*     docker compose up
* Open your chatbot and ask the following question : `What is the name of the movie with a storyline set on the planet "Mongo"?`
* It should return `Flash Gordon`. It means, your AI Chatbot is working!
* Try different questions too!

![image](https://github.com/Pragna235/Anythink-Market-43lr7kae/assets/109524200/33945d88-0bc9-4e2a-8b71-38fa297c8b6f)


![image](https://github.com/Pragna235/Anythink-Market-43lr7kae/assets/109524200/e157b2e4-56b1-4521-adc2-201d00bca412)

![image](https://github.com/Pragna235/Anythink-Market-43lr7kae/assets/109524200/06e32e88-a8e0-4a3d-850b-126410b80a3f)



