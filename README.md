# RSBP_241216_FP
Proyek akhir mata kuliah Rekayasa Sistem Berbasis Pengetahuan

**Proposal for Implementing SPARQL in Smart Home Systems**

- Faiz Haq Noviandra CP 5025211132
- Muhammad Arkan Karindra D. 5025211236
- Muhammad Febriansyah 5025211164
- Immanuel Pascanov Samosir 5025211257
- Radhiyan Muhammad Hisan 5025211166
 

---

### 1. **Introduction**
The integration of smart home systems with semantic web technologies provides robust solutions for managing and automating home devices. By leveraging Resource Description Framework (RDF), SPARQL, and ontologies, we can establish a Semantic Network for efficient device control and data processing. This proposal outlines the implementation process, tools required, and technology comparisons for achieving this integration.

---

### 2. **Objective**
To develop a smart home system using semantic technologies, enabling:
1. RDF-based device data management.
2. Execution of SPARQL queries for device control and monitoring.
3. Dynamic interaction between IoT devices and semantic networks.
4. Real-time communication and visualization of sensor data.

---

### 3. **Scope of Work**
This proposal focuses on the following tasks:
1. Setting up Apache Jena Fuseki as the RDF triple store.
2. Creating an RDF dataset in Turtle (.ttl) format.
3. Validating the RDF dataset using Python libraries.
4. Querying and uploading RDF data using SPARQL and Python.
5. Developing Python-based real-time communication and visualization systems.
6. Comparing Python and JavaScript as back-end development tools for smart home systems.

---

### 4. **Technology Comparison: Python vs. JavaScript**

#### 4.1 **Ecosystem and Libraries**
- **JavaScript**: 
  - Strong for web backend development with frameworks like Express.js.
  - Extensive libraries for lightweight and fast API development.
  - Can be used for full-stack development (Node.js for backend and frontend).
  - Limited built-in libraries for SPARQL or RDF compared to Python.
- **Python**: 
  - Supports RDF and SPARQL through libraries like RDFlib, SPARQLWrapper, and PySHACL for RDF validation.
  - Ideal for integration with Machine Learning and Data Analysis using libraries like scikit-learn, TensorFlow, and Pandas.
  - Frequently used in semantic web and data-driven projects.

#### 4.2 **Performance**
- **JavaScript**: 
  - Efficient for asynchronous requests.
  - Suitable for APIs requiring real-time communication with IoT devices using protocols like WebSocket or MQTT.
  - Faster project startup due to Node.js’s non-blocking nature.
- **Python**:
  - Superior for computational-heavy tasks like data analysis and machine learning.
  - Slightly slower for I/O-intensive tasks but excels in RDF and SPARQL management.

#### 4.3 **Triple Store Compatibility**
- **JavaScript**:
  - Libraries like sparql-http-client allow communication with triple stores such as Apache Jena Fuseki.
  - Requires additional libraries or manual approaches for deep RDF support.
- **Python**:
  - Comprehensive RDF support via libraries like RDFlib.
  - Strong ecosystem for direct RDF/OWL file manipulation.

#### 4.4 **Ease of Development**
- **JavaScript**:
  - Full-stack development is easier if JavaScript is already used for frontend.
  - Excels in real-time applications, suitable for IoT.
- **Python**:
  - Developer-friendly for beginners, especially for data and algorithm tasks.
  - Vast community support for data analysis, AI, and RDF management.

#### 4.5 **Use Case Specificity**
- **JavaScript**: Best suited for:
  - Real-time IoT applications.
  - Full-stack projects requiring seamless backend and frontend integration.
  - Lightweight REST APIs.
- **Python**: Best suited for:
  - Semantic data-heavy systems (ontology-focused).
  - Projects integrating machine learning or data analysis.
  - In-depth RDF/OWL tasks (validation, reasoning).

#### 4.6 **Recommendation**
- Use **JavaScript (Node.js)** for:
  - Lightweight, responsive APIs.
  - Real-time communication between IoT devices.
  - Full-stack web development.
- Use **Python** for:
  - RDF/OWL-intensive projects.
  - Machine learning integration.
  - Complex semantic data manipulation and reasoning.

---

### 5. **Implementation Plan**

#### 5.1 **Environment Setup**
- Install **Apache Jena Fuseki** as the RDF server.
- Install **Python 3.x** and required libraries:
  ```
  pip install sparqlwrapper rdflib requests
  ```

#### 5.2 **RDF Dataset Creation**
- Create a Turtle (.ttl) file defining smart home devices and their relationships.

Example Dataset:
```turtle
@prefix ex: <http://example.org/smart-home#> .
@prefix rdf: <http://www.w3.org/1999/02/22-rdf-syntax-ns#> .

ex:BedroomFan rdf:type ex:Device ;
              ex:hasType "Fan" ;
              ex:isLocatedIn ex:Bedroom ;
              ex:controlledBy ex:TemperatureSensor1 .
```

#### 5.3 **Validation**
- Validate the dataset using Python’s **rdflib** library:
  ```python
  from rdflib import Graph

  def validate_rdf(file_path):
      try:
          g = Graph()
          g.parse(file_path, format="turtle")
          print("RDF file is valid.")
      except Exception as e:
          print(f"Validation failed: {e}")
  validate_rdf("smart-home.ttl")
  ```

#### 5.4 **Uploading RDF Data**
- Use **requests** to upload RDF data to Fuseki:
  ```python
  import requests

  DATASET_URL = "http://localhost:3030/smart-home/data"

  def upload_rdf(file_path):
      with open(file_path, 'rb') as rdf_file:
          headers = {'Content-Type': 'text/turtle'}
          response = requests.post(DATASET_URL, data=rdf_file, headers=headers)
          if response.status_code == 200:
              print("Upload successful.")
          else:
              print(f"Upload failed: {response.status_code}")
  upload_rdf("smart-home.ttl")
  ```

#### 5.5 **SPARQL Querying**
- Write SPARQL queries to retrieve and manipulate RDF data.

Example Query:
```sparql
PREFIX ex: <http://example.org/smart-home#>
SELECT ?device ?type ?location
WHERE {
    ?device a ex:Device .
    OPTIONAL { ?device ex:hasType ?type . }
    OPTIONAL { ?device ex:isLocatedIn ?location . }
}
```

- Execute the query in Python using **SPARQLWrapper**:
  ```python
  from SPARQLWrapper import SPARQLWrapper, JSON

  SPARQL_ENDPOINT = "http://localhost:3030/smart-home/sparql"

  def run_query(query):
      sparql = SPARQLWrapper(SPARQL_ENDPOINT)
      sparql.setQuery(query)
      sparql.setReturnFormat(JSON)
      try:
          results = sparql.query().convert()
          return results["results"]["bindings"]
      except Exception as e:
          print(f"Error: {e}")
          return []
  ```

#### 5.6 **Real-Time Communication and Visualization**
- Implement WebSocket-based communication:
  ```python
  import asyncio
  import websockets

  async def echo(websocket, path):
      async for message in websocket:
          await websocket.send(f"Received: {message}")

  asyncio.get_event_loop().run_until_complete(websockets.serve(echo, "localhost", 8765))
  asyncio.get_event_loop().run_forever()
  ```

- Create real-time dashboards using **Dash** or **Plotly**.

---

### 6. **Semantic Network Integration**
The system is classified under **Semantic Networks** due to:
1. RDF-based data representation using triples.
2. SPARQL queries for structured data retrieval.
3. Ontologies defining relationships between entities.

---

### 7. **Conclusion and Future Work**
This proposal establishes the groundwork for a smart home system using RDF, SPARQL, and Python. Future enhancements include:
1. Dynamic updates to RDF from real-time data.
2. Integration of Case-Based Reasoning (CBR) for decision-making.
3. Implementation of optimization algorithms for energy efficiency.
4. Development of a user-friendly web interface.

By leveraging semantic technologies, this system ensures scalability, flexibility, and intelligent control for smart homes.

