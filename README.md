# Job Search Agent

I built this Python project to find part-time work alongside my physics studies. It searches for vacancies in German and English around Zürich and St. Gallen and checks whether I could actually get to work by train.

## How it works

- **Tavily** finds vacancies and retrieves the advert text.
- **Ollama** runs Qwen locally to read adverts, compare the duties with a search profile and help choose which results to explore.
- **The Swiss public transport API** looks up a weekday connection from St. Gallen station to the advertised workplace. Python checks the 120-minute limit.

The current search targets a February 2027 start and workloads that overlap with 40 to 60%. The HTML report separates confirmed matches from jobs with open questions. Adverts without a verifiable work address can be missed.

## Run locally

You need Ubuntu (including Ubuntu on WSL), Python 3.9 or newer, Ollama and a Tavily API key. From the repository folder, run:

```bash
ollama pull qwen3:4b
cp -n profile.example.txt profile.txt
python3 agent.py --setup
python3 agent.py
```

Setup prompts for your Tavily key. Edit `profile.txt` to change the skills and roles you are looking for. The region, commute limit, workload range and target month are set in `agent.py`.

Open `latest.html` in a browser to read the results. Each job title links to its source. The same run also produces `latest.csv`.
