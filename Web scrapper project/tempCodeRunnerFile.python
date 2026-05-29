import requests
from bs4 import BeautifulSoup
import pandas as pd


class JobScraper:

    def __init__(self, url):
        self.url = url
        self.jobs = []

    def fetch_page(self):
        try:
            response = requests.get(self.url, timeout=10)

            if response.status_code == 200:
                return response.text

            print(f"Failed! Status Code: {response.status_code}")
            return None

        except requests.exceptions.RequestException as e:
            print("Connection Error:", e)
            return None

    def parse_jobs(self, html):
        soup = BeautifulSoup(html, "html.parser")

        job_cards = soup.find_all("div", class_="card-content")

        for job in job_cards:

            title = job.find("h2", class_="title")

            company = job.find("h3", class_="company")

            location = job.find("p", class_="location")

            self.jobs.append({
                "Title": title.text.strip() if title else "N/A",
                "Company": company.text.strip() if company else "N/A",
                "Location": location.text.strip() if location else "N/A"
            })

    def save_csv(self, filename="jobs.csv"):
        df = pd.DataFrame(self.jobs)
        df.to_csv(filename, index=False)
        print(f"\nSaved {len(self.jobs)} jobs to {filename}")

    def filter_jobs(self, keyword):
        results = []

        for job in self.jobs:
            if keyword.lower() in job["Title"].lower():
                results.append(job)

        return results


def main():

    url = "https://realpython.github.io/fake-jobs/"

    scraper = JobScraper(url)

    html = scraper.fetch_page()

    if html:

        scraper.parse_jobs(html)

        scraper.save_csv()

        print("\nAvailable Jobs:")
        print("-" * 50)

        for job in scraper.jobs[:10]:
            print(f"Title: {job['Title']}")
            print(f"Company: {job['Company']}")
            print(f"Location: {job['Location']}")
            print("-" * 50)

        keyword = input("\nEnter keyword to search jobs: ")

        results = scraper.filter_jobs(keyword)

        print(f"\nFound {len(results)} matching jobs\n")

        for job in results:
            print(job)


if __name__ == "__main__":
    main()