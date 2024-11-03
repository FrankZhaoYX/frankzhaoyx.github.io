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
- **BeautifulSoup** – Used for web scraping, specifically to retrieve each movie's director information.
```javascript
        raw = mv_info.find_all('li', attrs={'data-testid': 'title-pc-principal-credit'})
        # Scrape the Director
        # director_raw = mv_info.find('a', class_='ipc-metadata-list-item__list-content-item ipc-metadata-list-item__list-content-item--link')
        # print(director_raw.get_text())
        try:
            director_raw = raw[0].find_all('a', class_="ipc-metadata-list-item__list-content-item ipc-metadata-list-item__list-content-item--link")
            director_set = [tmp.get_text() for tmp in director_raw]
        except:
            director_set=""
        # print(director_set)
        self.directors.append(director_set)
```
- **Requests** – Used to retrieve webpage data for each specified year, with error handling implemented to manage potential exceptions.```javascript

    def bs4_parser(self, url):
        try:
            # Make the GET request
            response = requests.get(url, headers=self.headers)

            # Raise an exception for HTTP errors
            response.raise_for_status()  # This will raise an HTTPError for bad responses (4xx, 5xx)

            # Process the response
            logging.info("Request was successful!")
            return BeautifulSoup(response.text, 'html.parser')
                    
        except requests.exceptions.HTTPError as http_err:
            logging.critical("HTTP error occurred: {}".format(http_err))  # e.g., 404 or 500 error
        except requests.exceptions.ConnectionError:
            logging.critical("Connection error occurred. Please check your network.")
        except requests.exceptions.Timeout:
            logging.critical("The request timed out.")
        except requests.exceptions.RequestException as err:
            logging.critical("An error occurred: {}".format(err))

```



### 3. Support the selection of appropriate statistical tools and techniques

<img src="images/dummy_thumbnail.jpg?raw=true"/>

### 4. Provide a basis for further data collection through surveys or experiments

Sed ut perspiciatis unde omnis iste natus error sit voluptatem accusantium doloremque laudantium, totam rem aperiam, eaque ipsa quae ab illo inventore veritatis et quasi architecto beatae vitae dicta sunt explicabo. 

For more details see [GitHub Flavored Markdown](https://guides.github.com/features/mastering-markdown/).
