This is a small repository of notebooks made when participating in the Kaggle LLM Prompt Recovery competition https://www.kaggle.com/competitions/llm-prompt-recovery.
The aim was to find what prompt was used in each case which made Gemma transform the given original text to the given rewritten one.
Immediately after starting, our main focus was to instruction tune an existing LLM. The best one based on others' submissions seemed to be the Mistral 7b parameter instruction tuned model.
We used a big dataset collected from other users which consisted of the prompt, original text, and a rewritten text output from Gemma. Using LoRA, we wanted to fine-tune the model to perform better on this task.
After successfully setting up the training notebook Colab_Training and inference/submission notebook Kaggle_Inference, we, unfortunately, found out that task-specific instruction tuning doesn't really help with the 
already instruction-tuned model for this task.
While this might have been due to the fact that Colab resources only let us train on a few hundred examples, we also found that training loss converged very quickly with the low batch sizes we had. So we wanted to at least 
maximize the possible batch size and by using methods like gradient accumulation, gradient checkpointing, bfloat16 datatype, etc ., we managed to get to 8 batch size working for T4 and 32 working for the A100 GPU. 
We noticed that this helped to get our training loss to go lower. But it still only maybe marginally helped in the competition.

In the end, it turned out that prompt formatting was clearly much more important in this case. We ended up placing narrowly in the first half in the final leaderboard due to using a template from a shared public notebook
(all credits given in the notebooks). Best scores required very specific tricks like concatenating multiple outputs or even using a token 'lucrarea' to adversarially attack the scoring method.

We would have likely done more work for instruction tuning and thought of adding a post-processing method to score and compare Gemma outputs based on prompts, but due to the former not helping with performance and the latter not being possible
due to the submission notebook time limit, these notebooks represent what we finally submitted. We could have done more work to tune prompts rather than models but thought that this work was practically more useful for us.
