# Job Search Agent

I built this tool to find part-time work alongside my physics studies in Switzerland. A search for “working student” can miss a sensor internship where most of the work is analysing data in Python. The agent searches for both job titles and the work itself, then ranks individual adverts for me to review.

## From a search result to a vacancy

1. Tavily searches Swiss job boards including ostjob.ch, jobs.ch, JobScout24 and Indeed, then searches the wider web in German and English. When a result is a listing page, Python follows promising links to individual adverts.
2. Python reads the advert and its structured job data. It looks for the advertised workload, handles ranges such as 40 to 80%, and can follow a link to the original listing. Full-time roles receive a strong ranking penalty. Missing details stay visible as open questions.
3. The agent groups likely duplicates using vacancy references, employer, location, title and text. When it recognises both versions, it prefers the employer link. Similar roles can still be hard to distinguish when publishers rewrite the advert.
4. Ollama runs Qwen locally for additional query suggestions and a closer review of selected vacancies. Where the workplace address can be verified, the agent can check a weekday connection from St. Gallen using the Swiss public transport API.

The ranked results appear in `latest.html` and `latest.csv` during the run. Each title links to the advert. I can record an application locally and see the date when the agent recognises the vacancy again. The agent does not send applications.

## Run on Ubuntu or WSL

Install Python 3.9 or newer and Ollama, and obtain a Tavily API key. Run these commands from the repository folder:

```bash
ollama pull qwen3:4b
cp -n profile.example.txt profile.txt
python3 agent.py --setup
python3 agent.py --quick
```

Open `latest.html` to review the results. Use `python3 agent.py` for the regular search. To record an application, run `python3 agent.py --applied` and paste the advert URL when prompted. The application list is exported to `bewerbungen.csv`.

Edit `profile.txt` to describe your skills and interests. The keyword weights, Swiss locations, target workload of 40 to 60% and February 2027 start are configured in `agent.py`. Keep `swiss_places.tsv` alongside the script. Search requests go to Tavily. The model runs locally, and the application history is stored in SQLite on your machine.

