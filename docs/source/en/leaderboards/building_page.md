# Building a leaderboard using a template

To build a leaderboard, the easiest is to look at our demo templates [here](https://huggingface.co/demo-leaderboard-backend)

## 📏 Contents

Our demo leaderboard template contains 4 sections: two spaces and two datasets.

- The `frontend space` displays the results to users, contains explanations about evaluations, and optionally can accept model submissions. 
- The `requests dataset` stores the submissions of users, and the status of model evaluations. It is updated by the frontend (at submission time) and the backend (at running time).
- The `results dataset` stores the results of the evaluations. It is updated by the backend when evaluations are finished, and pulled by the frontend for display.
- The `backend space` is optional, if you run evaluations manually or on your own cluster. It looks at currently pending submissions, and launches their evaluation using either the Eleuther AI Harness (`lm_eval`) or HuggingFace's `lighteval`, then updates the evaluation status and stores the results. It needs to be edited with your own evaluation suite to fit your own use cases if you use something more specific.

## 🪛 Getting started

You should copy the two spaces and the two datasets to your org to get started with your own leaderboard!

### Setting up the frontend

To get started on your own frontend leaderboard, you will need to edit 2 files:
- src/envs.py to define your own environment variable (like the org name in which this has been copied)
- src/about.py with the tasks and number of few-shots you want for your tasks

### Setting up fake results to initialize the leaderboard

Once this is done, you need to edit the "fake results" file to fit the format of your tasks: in the sub dictionary `results`, replace task_name1 and metric_name by the correct values you defined in tasks above.
```
"results": {
    "task_name1": {
        "metric_name": 0
    }
}
```

At this step, you should already have some results displayed in the frontend!

Any more model you want to add will need to have a file in request and one in result, following the same template as already present files.

### Optional: Setting up the backend

If you plan on running your evaluations on spaces, you then need to edit the backend to run the evaluations most relevant for you in the way you want. 
Depending on the suite you want to learn, this is the part which is likely to take the most time.

However, this is optional if you only want to use the leaderboard to display results, or plan on running evaluations manually/on your own compute source.

## 🔧 Tips and tricks

Leaderboards setup in the above fashion are adjustable, from providing fully automated evaluations (a user submits a model, it is evaluated, etc) to fully manual (every new evaluation is run with human control) to semi-automatic. 

When running the backend in Spaces, you can either :
- upgrade your backend space to the compute power level you require, and run your evaluations locally (using `lm_eval`, `lighteval`, or your own evaluation suite); this is the most general solution across evaluation types, but it will limit you in terms of model size possible, as you might not be able to fit the biggest models in the backend
- use a suite which does model inference using API calls, such as `lighteval` which uses `inference-endpoints` to automatically spin up models from the hub for evaluation, allowing you to scale the size of your compute to the current model.

If you run evaluations on your own compute source, you can still grab some of the files from the backend to pull and push the `results` and `request` datasets.

Once your leaderboard is setup, don't forget to set its metadata so it gets indexed by our Leaderboard Finder. See "What do the tags mean?" in the [LeaderboardFinder](https://huggingface.co/spaces/leaderboards/LeaderboardFinder) space.
