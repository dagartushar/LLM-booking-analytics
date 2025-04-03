# LLM-booking-analytics
Requirements
Before you run the project, make sure you have the following Python libraries installed:

bash
Copy
pip install sentence_transformers
pip install faiss-cpu
pip install kaggle
pip install pandas
pip install flask
pip install scikit-learn
Setup
Download the Dataset:

The dataset used in this project is a hotel bookings CSV file. You need to place the CSV file (e.g., hotel_bookings.csv) in the specified file path ('C:\\Users\\admin\\OneDrive\\Desktop\\New folder (2)\\hotel_bookings.csv').

Ensure that the dataset contains columns like adr, lead_time, hotel, arrival_date_year, arrival_date_month, arrival_date_day_of_month, is_canceled, and country.

Preprocessing:

The code will clean the dataset by removing rows with missing critical information (ADR, lead time, hotel name, and arrival month).

It also combines year, month, and day into a datetime column (arrival_date).

Embedding Generation:

The SentenceTransformer model ('all-MiniLM-L6-v2') is used to generate embeddings for rows in the dataset based on key features like hotel name, country, arrival date, and ADR.

FAISS Indexing:

The generated embeddings are indexed using FAISS for fast similarity search. This allows you to ask questions and find the most relevant data entries quickly.

Flask API:

The code sets up a Flask API with two main endpoints:

/analytics - Returns various analytics reports like revenue trends, cancellation rate, and geographical distribution.

/ask - Accepts a query and returns the most relevant row from the dataset using the question-answering system.

How to Run
Place the hotel bookings dataset in the specified file path.

Run the following command to start the Flask API server:

bash
Copy
python app.py
The server will be running at http://127.0.0.1:5000/. You can access the API endpoints via the following routes:

Analytics Reports:

Endpoint: POST /analytics

Response: JSON containing revenue trends, cancellation rate, geographical distribution, and lead time distribution.

Ask a Question:

Endpoint: POST /ask

Request Body:

json
Copy
{
  "query": "What is the average lead time for bookings from Portugal?"
}
Response: JSON with the most relevant information from the dataset.

Example Requests
Analytics Request
POST to /analytics:

json
Copy
{
  // No body content needed
}
Response:

json
Copy
{
  "revenue_trends": {
    "2023-01": 123456.78,
    "2023-02": 98765.43
  },
  "cancellation_rate": 5.2,
  "geographical_distribution": {
    "PRT": 1500,
    "ESP": 1200
  },
  "lead_time_distribution": {
    "count": 1000,
    "mean": 50.2,
    "std": 15.5
  }
}
Question Request
POST to /ask:

json
Copy
{
  "query": "Which hotel has the highest average daily rate?"
}
Response:

json
Copy
{
  "hotel": "Hotel A",
  "arrival_month": "July",
  "country": "USA",
  "lead_time": 30,
  "adr": 350.5
}
How It Works
Data Loading: The CSV file is loaded and cleaned, removing entries with missing critical data.

Embedding Generation: Relevant data columns are converted into text and transformed into embeddings using the SentenceTransformer model.

Similarity Search: FAISS is used to index the embeddings, making it possible to efficiently find the most similar data points to any query.

Question Answering: When a query is posted to the /ask endpoint, the system converts the query to an embedding and finds the most similar dataset row based on cosine similarity.

This project is open-source and available under the MIT License.
