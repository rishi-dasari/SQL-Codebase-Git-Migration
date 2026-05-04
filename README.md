# Enterprise SQL Codebase Git Migration
 
## The Problem
 
The database team at this company managed 4,000 SQL objects — stored procedures, views, functions, tables — from a shared network drive. No version control. No history. When two developers worked on the same stored procedure, the last one to save won. When a bad query hit production, the fix was reconstructing the original logic from memory.
 
I was brought in to fix that.
 
## What I Built
 
I wrote a migration script that extracted all 4,000 `.sql` files from the shared drive and reorganized them into a clean repository structure (`/Tables`, `/Views`, `/StoredProcedures`). That became the first commit.
 
From there, I set up the workflow the team would actually use going forward. Direct pushes to `main` are blocked. Every database change goes through a feature branch and requires at least one peer review before it merges. Releases are tagged with semantic versioning (`v1.0.0`, `v1.1.0`, etc.), so if a deployment fails, reverting to the last stable state is one command: `git checkout v1.0.0`.
 
I chose Trunk-Based Development over Git Flow because the team makes frequent small changes to shared database objects. Long-lived branches would have just recreated the clobbering problem in a different form.
 
## What Changed
 
Rollback time went from hours of manual debugging to under a minute. Merge conflicts are caught automatically during PR review instead of silently overwriting each other on a shared drive. Every change now has a record of who approved it and when — which matters when auditors ask.
 
## Run the Simulation
 
The `simulate_migration.py` script in this repo recreates the full scenario in any Python environment, including Google Colab. It generates 4,000 mock SQL objects, runs the initial migration commit, simulates a feature branch edit and PR merge, and walks through a rollback using Git tags. It's useful for seeing the workflow in motion before adapting it to a real codebase.
