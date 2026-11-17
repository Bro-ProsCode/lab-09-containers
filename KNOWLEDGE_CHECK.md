# Lab 09: Knowledge check

**Answer each question below it, in your own words, in one to three sentences.** Replace the `...` below **Your answer:** with your text.

## 1. Credentials in the image?

Assume you want to share your image with others and make it publicly available on `ghcr.io`, so that anyone can pull it and run the analysis. A teammate suggests adding `COPY .aws /home/app/.aws` to the `Dockerfile`, so that nobody has to type `-v ~/.aws:/home/app/.aws:ro` any more. (This is a thought experiment: keep your own package private.)

**Your answer:** Give two reasons why this is a bad idea. Think of who could then read your credentials, and of what happens when your Learner Lab session ends.

This is a bad idea because it allows anyone with the image to view your credentials and login as you elsewhere. It also makes the credentials invalid once the Learner Lab session ends since the container isn't mounting from your live session.

## 2. Where did the Atlas password come from?

In Step 4, you ran the analysis once with `-v ~/.aws:/home/app/.aws:ro` only, without the three `-e` options. The container still wrote to your Atlas cluster.

*Hint:* Read `get_atlas_settings()` in `src/book_analysis/analyze.py`.

**Your answer:** Explain, step by step, where the container found the Atlas settings in that run.

Analyze.py first tried to get into Mongo using the enviornment variables. Upon those failing (since they weren't passed in), the script successfully accessed them in via AWS' SSM paremeter, allowing it to write to the database/cluster since the disk was mounted.

## 3. Same tag, different image

Run `docker images book-analysis`, and note the image ID of `book-analysis:0.1.0`. Then change the `description` in the `LABEL` of your `Dockerfile`, build again with the same command, `docker build -t book-analysis:0.1.0 .`, and run `docker images book-analysis` again.

**Your answer:** Paste the image ID from before and after the rebuild. Why is it a problem for someone who uses your image that `0.1.0` now names a different image, and what should you have done instead?

Before: bacbc118b175  
After: 1409675661b6  
  
They are different because the hashing algorithm evaluates to different hashes since the binaries are different. This type of overwriting would break the entire point of containers for someone expecting a certain version since the file has chnaged but the tag has not.
  
I should have just pushed a new tag with my updated description.
