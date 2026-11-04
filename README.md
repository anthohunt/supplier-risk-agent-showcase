**English** · [Français](README.fr.md)

> Showcase repository: the code and data of this project are private; this README describes it.

# Supplier risk agent

A small 0.8-billion-parameter model that reads the news and flags, offline, the articles that announce a risk for a supplier.

## The problem

A procurement team needs to know quickly when one of its suppliers is hit by an incident that can slow its deliveries: a production stop, a strike, a lawsuit, financial trouble, a shortage, a cyberattack. This project is a client case: the agent follows a list of 94 suppliers and must run on a laptop, without sending data outside.

## How it works

- Articles come from the GDELT global news feed; the agent reads the full article.
- It looks for a listed supplier that is hit by a disruptive event still under way, among six types: production, labour, legal, financial, price or shortage, cyber.
- Code checks the source: the site's domain must be in a closed list of 2,033 reliable domains.
- Decision: "risk" if the signal comes from a reliable source, "to check" otherwise (a human decides), "no risk" without a signal.
- The model's weights are not changed. Only the harness around the model (questions asked, splitting, rules) is improved, by La boucle: a researcher agent proposes trials, each one is measured, and a sealed exam decides.

## What is built

- Test bench balanced across the three decisions: a sealed exam of 240 articles, a verification set of 240 articles. Pass mark set at 224 correct out of 240.
- Training measured on 1,020 articles: from 76.18% correct decisions for the model alone to 90.39% (922 out of 1,020) with the best harness.
- First exam: 205 correct out of 240 (85.4%), below the pass mark. Cause: overfitting to the training articles, hence the addition of a hidden control set.
- Standalone Windows application, in English: list of articles with their verdict, alert on a risk, and the agent's reasoning. Total size 2.57 GB (model 1.52 GB, engine 0.94 GB, application 0.11 GB). On 100 articles, decisions identical to the bench's; 3.97 s per article on average.

Status: paused. The second exam has not been taken.

## What is public

This page only. The code, the data and the application are a confidential client deliverable.

## Tech stack

- Kev-0.8B, an open-source decision model based on Qwen3.5-0.8B
- Python, PyTorch and transformers for training
- llama.cpp (CUDA) as the application's engine
- PyInstaller for the Windows executable
- GDELT for the article feed
