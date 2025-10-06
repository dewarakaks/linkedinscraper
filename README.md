---
## UPDATE Oktober 2025
- Menambahkan Dockerfile untuk menjalankan scraper secara otomatis di container
- Menambahkan docker-compose.yml agar scraping otomatis dan database Postgres siap reporting
- Scraper dapat dijalankan tanpa web app, hasil data bisa diekspor langsung ke Postgres (siap untuk visualisasi BI/Looker)
- Contoh penggunaan:
   - `docker-compose up --build` untuk menjalankan full pipeline
   - Data hasil scraping siap direporting ke BI tools
---

# UPDATE August 2023.
New version includes OpenAI integration for cover letter generation. See below for how to configure config.json file.
## LinkedIn Job Scraper
This is a Python application that scrapes job postings from LinkedIn and stores them in a SQLite database. The application also provides a web interface to view the job postings and mark them as applied, rejected,interview, and hidden.
![Screenshot image](./screenshot/screenshot1.png)
### Problem
If you spent any amount of time looking for jobs on LinkedIn you know how frustrating it is. The same job postings keep showing up in your search results, and you have to scroll through pages and pages of irrelevant job postings to find the ones that are relevant to you, only to see the ones you applied for weeks ago. This application aims to solve this problem by scraping job postings from LinkedIn and storing them in a SQLite database. You can filter out job postings based on keywords in Title and Description (tired of seeing Clinical QA Manager when you search for software QA jobs? Just filter out jobs that have "clinical" in the title). The jobs are sorted by date posted, not by what LinkedIn thinks is relevant to you. No sponsored job posts. No duplicate job posts. No irrelevant job posts. Just the jobs you want to see.
### IMPORTANT NOTE
If you are using this application, please be aware that LinkedIn does not allow scraping of its website. Use this application at your own risk. It's recommended to use proxy servers to avoid getting blocked by LinkedIn (more on proxy servers below).
### Prerequisites
- Python 3.6 or higher
- Flask
- Requests
- BeautifulSoup
- Pandas
- SQLite3
- Pysocks
### Installation
1. Clone the repository to your local machine.
2. Install the required packages using pip: `pip install -r requirements.txt`
3. Create a `config.json` file in the root directory of the project. See the `config.json` section below for details on the configuration options. Config_example.json is provided as an example, feel free to use it as a template.
4. Run the scraper using the command `python main.py`. Note: run this first first to populate the database with job postings prior to running app.py.
4. Run the application using the command `python app.py`.
5. Open a web browser and navigate to `http://127.0.0.1:5000` to view the job postings.
### Usage
The application consists of two main components: the scraper and the web interface.
#### Scraper
The scraper is implemented in `main.py`. It scrapes job postings from LinkedIn based on the search queries and filters specified in the `config.json` file. The scraper removes duplicate and irrelevant job postings based on the specified keywords and stores the remaining job postings in a SQLite database.
To run the scraper, execute the following command:
```
python main.py
```
#### Web Interface
The web interface is implemented using Flask in `app.py`. It provides a simple interface to view the job postings stored in the SQLite database. Users can mark job postings as applied, rejected, interview, or hidden, and the changes will be saved in the database.
When the job is marked as "applied" it will be highlighted in light blue so that it's obvious at a glance which jobs are applied to. "Rejecetd" will mark the job in red, whereas "Interview" will mark the job in green. Upon clicking "Hide" the job will dissappear from the list. There's currently no functionality to reverse these actions (i.e. unhine, un-apply, etc). To reverse it you'd have to go to the database and change values in applied, hidden, interview, or rejected columns.
