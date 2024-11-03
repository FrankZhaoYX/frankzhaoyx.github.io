## Imdb Analytic Project

**Project description:** This project aimed to mine movie data from 2000 to 2021, capturing details such as movie name, release year, genre, IMDb rating, votes, and Metascore. After data extraction, I utilized Tableau to analyze the dataset and generate an interactive dashboard, highlighting trends and insights within the film industry over two decades.


### 1. Main Techniques

- **BeautifulSoup** – Web scraping
- **Requests** – Data retrieval from web sources
- **Pandas** – Data cleaning and manipulation
- **MySQL** – Database storage and querying
- **Docker** – Containerization for environment setup
- **Tableau** – Data visualization and dashboard creation



### 2. Project details
- **BeautifulSoup** – Used for web scraping, specifically to retrieve each movie's name, and year.
```python
  def scrape_mv(self, container):
      # Scrape the name
      name = container.find('h3', class_ = 'ipc-title__text').get_text()
      self.names.append(name)
      logging.info(f"Current is scraping movie {name}")
      # Scrape the metadata
      # metadata_set = container.find_all('span', class_ = 'sc-b189961a-8 hCbzGp dli-title-metadata-item')
      metadata_set = container.find_all('span', class_ = "sc-ab348ad5-8 cSWcJI dli-title-metadata-item")
      year = metadata_set[0].get_text()
      self.years.append(year)
```
- **Requests** – Utilized for retrieving webpage data for each specified year, with robust error handling to manage potential exceptions such as HTTP errors, connection issues, and timeouts. The `bs4_parser` function below demonstrates this setup, where requests are made, errors are logged, and successful responses are parsed with BeautifulSoup:

```python
  def bs4_parser(self, url):
      try:
          # Make the GET request
          response = requests.get(url, headers=self.headers)
          response.raise_for_status()  # Raises an HTTPError for bad responses (4xx, 5xx)
          
          # Process the response
          logging.info("Request was successful!")
          return BeautifulSoup(response.text, 'html.parser')
                  
      except requests.exceptions.HTTPError as http_err:
          logging.critical(f"HTTP error occurred: {http_err}")  # e.g., 404 or 500 error
      except requests.exceptions.ConnectionError:
          logging.critical("Connection error occurred. Please check your network.")
      except requests.exceptions.Timeout:
          logging.critical("The request timed out.")
      except requests.exceptions.RequestException as err:
          logging.critical(f"An error occurred: {err}")

      return None  # Return None if request fails
```
- **Pandas** – Used to transform and clean the data, then export it to CSV format to serve as the data source for injection into the MySQL server. This process ensures the data is well-structured and ready for efficient storage and querying within the database.
```python
        mv_containers = page_html.find_all('li', class_ = 'ipc-metadata-list-summary-item')
        # print(len(mv_containers))
        for container in mv_containers:
            self.scrape_mv(container)
            # break

        movie_raw = pd.DataFrame(
        {   'movie': self.names,
            'year': self.years,
            'length': self.lengths,
            'genres': self.genres,
            'US certificates': self.rating_years,
            'imdb': self.imdb_ratings,
            'metascore': self.metascores,
            'votes': self.votes,
            'reviews': self.reviews,
            'directors': self.directors,
            'writers': self.writers,
            'stars': self.stars,
            'contents': self.contents
        })
```
- **MySQL** – Used to import the CSV data into the MySQL server as part of the bronze layer, where raw data is stored. Additionally, MySQL is leveraged to transform and extract specific data into a silver layer, representing a more refined dataset ready for analysis.

```python
    #  Step 2: Now, create the silver_dataset table based on this valid data
    # Create table `silver_dataset` with the fomulated structure 
    sql_tmp = """CREATE TABLE silver_dataset(
    movie varchar(255) NOT NULL,
    year int NOT NULL,
    length TIME NOT NULL,
    genre varchar(255) NOT NULL,
    US_certificates varchar(10) NOT NULL,
    imdb float NOT NULL,
    metascore int NOT NULL,
    votes int NOT NULL,
    reviews int NOT NULL,
    directors varchar(255) NOT NULL,
    writers varchar(255) NOT NULL,
    stars varchar(255) NOT NULL,
    contents varchar(255) NOT NULL
    );"""
    db_manager.create_table(sql_tmp,"silver_dataset")
```




### 3. Support the selection of appropriate statistical tools and techniques

<img src="images/dummy_thumbnail.jpg?raw=true"/>

### 4. Provide a basis for further data collection through surveys or experiments

Sed ut perspiciatis unde omnis iste natus error sit voluptatem accusantium doloremque laudantium, totam rem aperiam, eaque ipsa quae ab illo inventore veritatis et quasi architecto beatae vitae dicta sunt explicabo. 

For more details see [GitHub Flavored Markdown](https://guides.github.com/features/mastering-markdown/).
