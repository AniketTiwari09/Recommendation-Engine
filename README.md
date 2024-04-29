# Content-based Recommendation Engine

## Description

This is a production-ready, but very simple, content-based recommendation engine that computes similar items based on text descriptions. It comes with a sample data file (the headers of the input file are expected to be identical to the same file -- id, description) of 500 products so you can try it out.

It is a Flask-based REST webservice designed to be deployed to Heroku and relies on Anaconda for installation of the scientific computing dependencies, and Redis to store precomputed similarities.

Read the comments in `engine.py` to see how it works. It's very simple!

`web.py` contains the two endpoints:

- `/train`: Calls `engine.train()` which precomputes item similarities based on their descriptions in `sample-data.csv` using TF-IDF and cosine similarity.
- `/predict`: Given an `item_id`, returns the precomputed 'most similar' items.

Try it out! First, make sure you have a local Redis instance running. The engine expects to find Redis at `redis://localhost:6379`, but you can set `REDIS_URL` env var if you have it running elsewhere.

You'll also need Anaconda installed (a scientific distribution of Python). Create a new virtualenv with the needed dependencies:

**Activate Virtual Environment**:

```bash
source activate crec

python web.py

curl -X GET -H "X-API-TOKEN: FOOBAR1" -H "Content-Type: application/json; charset=utf-8" http://127.0.0.1:5000/train -d '{"data-url": "sample-data.csv"}'
curl -X POST -H "X-API-TOKEN: FOOBAR1" -H "Content-Type: application/json; charset=utf-8" http://127.0.0.1:5000/predict -d '{"item":18,"num":10}'
