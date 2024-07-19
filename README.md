# Self-Driving Car Reinforcement Learning
This project uses reinforcement learning (RL) in the OpenAI Gymnasium Car Racing Environment to teach the car how to drive on the track.


<div style="display: flex; flex-wrap: wrap; gap: 10px;">
  <img src="images/gifs/ppo_carracing_10000_video_v1.gif" width="200" height="200" alt="gif1">
  <img src="images/gifs/ppo_carracing_1000000_video_v1.gif" width="200" height="200" alt="gif2">
  <img src="images/gifs/ppo_carracing_v3_1000000_video.gif" width="200" height="200" alt="gif3">
  <img src="images/gifs/ppo_carracing_v4_490000_video.gif" width="200" height="200" alt="gif4">
</div>

## Models

Four models were trained using stable baselines Proximal Policy Optimization (PPO) and adjusting the hyperparameters in search of the best outcome.
![Rewards](images/training_logs/all/Xnip2024-07-18_15-47-12.jpg)


## HyperParameters
![Hyperparameters](images/training_logs/all/Xnip2024-07-18_13-29-18.jpg)

Changes:
![highlighted_changes](images/training_logs/all/Xnip2024-07-18_13-29-34.jpg)


### Learning Rate
I changed the learning rate from the default twice in the code, however TensorBoard showed an unchanged rate. No errors were thrown, so not sure what the issue was. Unfortunate because this is a main parameter and should have given me more control over the training.
![learning_rate](images/training_logs/all/Xnip2024-07-18_15-47-59.jpg)

### Clip Range
Controls the clipping of the policy probability ratios during the training process. This clipping is crucial for ensuring stable and reliable updates to the policy, preventing overly large updates that could destabilize training.
-   A smaller `clip_range` results in more conservative updates, potentially slowing down training but increasing stability. A larger `clip_range` allows for more significant updates but can risk instability.

I used the default and a formula which decreases the `clip_range` over time, linearly.
![clip_range](images/training_logs/all/Xnip2024-07-18_15-46-13.jpg)
Result:
![clipping](images/training_logs/all/Xnip2024-07-18_15-46-31.jpg)

### Batch Size
Determines how many experiences (combinations of state, action, reward, etc.) are used in a single forward and backward pass of the neural network.

Tried 32, 64 and 128.

### Epoch
The number of epochs determines how many times this batch is reused for updating the networks.

I used 10 and 20 for these parameters.

Passes before policy update:
 - v1: 10 x 64 = 640
 - v2: 10 x 64 = 640
 - v3: 20 x 128 =2560
 - v4: 10 x 32 = 320

### Validation Function Coefficient
In PPO, the combined loss function typically has three main components:

1.  **Policy Loss**: Encourages the policy to improve the expected return.
![policyloss](images/training_logs/all/Xnip2024-07-18_15-48-37.jpg)
2.  **Value Function Loss**: Ensures the value function accurately predicts the expected returns.
![vfloss](images/training_logs/all/Xnip2024-07-18_15-49-15.jpg)
3.  **Entropy Bonus**: Encourages exploration by penalizing deterministic policies.
![entropy](images/training_logs/all/Xnip2024-07-18_15-46-00.jpg)




The value function loss measures the difference between the predicted value by the critic and the actual returns observed. By adjusting the value function coefficient, you are essentially determining how much emphasis to put on improving the value function relative to the policy.

-   **High `vf_coef`**: More emphasis on the value function loss. This can lead to a more accurate value function but might slow down the policy updates.
-   **Low `vf_coef`**: Less emphasis on the value function loss. This can speed up policy updates but might lead to a less accurate value function.

Used:
- v1: 0.5
- v2: 0.5
- v3: 0.25
- v4: 0.3
I think this parameter may have had the greatest negative impact on later models.

Combined loss:
![combined_loss](images/training_logs/all/Xnip2024-07-18_15-48-14.jpg)


## Results

### Model 4
200,000 training steps
<div style="display: flex; flex-wrap: wrap; gap: 10px;">
  <img src="images/training_logs/all/Xnip2024-07-18_16-16-41.jpg" width="500" height="200" alt="gif3">
  <img src="images/gifs/ppo_carracing_v4_200000_video.gif" width="200" height="200" alt="gif4">
</div>

490,000 training steps
<div style="display: flex; flex-wrap: wrap; gap: 10px;">
  <img src="images/training_logs/all/Xnip2024-07-18_16-16-57.jpg" width="500" height="200" alt="gif3">
  <img src="images/gifs/ppo_carracing_v4_490000_video.gif" width="200" height="200" alt="gif4">
</div>
### Model 3
540,000 training steps
<div style="display: flex; flex-wrap: wrap; gap: 10px;">
  <img src="images/training_logs/" width="200" height="200" alt="gif3">
  <img src="images/gifs/ppo_carracing_v3_540000_video.gif" width="200" height="200" alt="gif4">
</div>

1,000,000 training steps
<div style="display: flex; flex-wrap: wrap; gap: 10px;">
  <img src="images/training_logs/" width="200" height="200" alt="gif3">
  <img src="images/gifs/ppo_carracing_v3_1000000_video.gif" width="200" height="200" alt="gif4">
</div>

### Model 2
930,000 training steps
<div style="display: flex; flex-wrap: wrap; gap: 10px;">
  <img src="images/training_logs/all/Xnip2024-07-18_16-17-57.jpg" width="500" height="200" alt="gif3">
  <img src="images/gifs/ppo_carracing_v2_930000_video.gif" width="200" height="200" alt="gif4">
</div>

### Model 1
10,000 training steps
<div style="display: flex; flex-wrap: wrap; gap: 10px;">
  <img src="images/training_logs/" width="200" height="200" alt="gif3">
  <img src="images/gifs/ppo_carracing_10000_video_v1.gif" width="200" height="200" alt="gif4">
</div>

460,000 training steps
<div style="display: flex; flex-wrap: wrap; gap: 10px;">
  <img src="images/training_logs/" width="200" height="200" alt="gif3">
  <img src="images/gifs/ppo_carracing_460000_video_v1.gif" width="200" height="200" alt="gif4">
</div>

1,000,000 training steps
<div style="display: flex; flex-wrap: wrap; gap: 10px;">
  <img src="images/training_logs/all/Xnip2024-07-18_16-18-17.jpg" width="500" height="200" alt="gif3">
  <img src="images/gifs/ppo_carracing_1000000_video_v1.gif" width="200" height="200" alt="gif4">
</div>


# Conclusions




