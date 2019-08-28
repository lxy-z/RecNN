<p align="center"> 
<img src="./res/logo.png">
</p>

This is my school project. It focuses on Reinforcement Learning, but there are many other things I learned during the development. Key topics: time series analysis, static dataset optimization, data preparation, and EDA. It also features my code for ML20 dataset that allows iterating through the dataset in a matter of 3 minutes. As well as my custom movie embeddings. DDPG doesn't seem to be working because it exploits the Value Network by recommending the same movie over and over again. But TD3 seems to be working just fine! You can see the distance matrices for the generated actions [below](#td3-results)

First article, the code is under notes/1. Vanilla RL/, it covers basic Reinforcement Learning Approach:

<p align="center"> 
   <a href="https://towardsdatascience.com/reinforcement-learning-ddpg-and-td3-for-news-recommendation-d3cddec26011">
        <img src="./res/Article.png">
    </a>
</p>

I am working on the next article, but the code I am writing is under notes/2. BCQ/. It is about RL applications to the static dataset (i.e. a dataset without exploration). It features adversarial reinforcement learning techniques. Right now the DDPG implementation is working.

Repos I used code from:
- Higgsfield's [RL Advemture 2](https://github.com/higgsfield/RL-Adventure-2)

- Sfujim's [BCQ](https://github.com/sfujim/BCQ)

- LiyuanLucasLiu [Radam](https://github.com/LiyuanLucasLiu/RAdam)

## Dataset Description
This project is built for MovieLens 20M dataset, but support for other datasets is in perspective. I have parsed all the movies in the '/links.csv' to get all auxiliary data from TMDB/IMDB. Text information was fed into Google's BERT/ OpenAI GPT2 models to get text embeddings. If you want to download anything, the links are down the description. 

Here is an overview:

- State - [None, frame_size * (embed_size+1) ] - PCA encoded previous actions (watched movies) embedding + rewards (ratings). All flattered and connected together
- Action - [None, embed_size] - PCA encoded current action embedding
- Reward - [None] - Integer, indicates whether the user liked the action or not
- Next state - look state - + Next state is basically the same but shifted +1 time step
- Done - [None] - Boolean, needed for TD(1)

## Misc Data

Everything of the misc sort is in the 'Misc Data' you can download in the downloads section, featuring all sorts of auxiliary stuff. Primarily it is movie info. If you don't want to use the embeddings, or just want to have some debug info/data for application this is what you need.

All text information is located in texts_bert.p / texts_gpt2.p in a dict {movie_id: numpy_array} format.

All of the categorical features had been label encoded, numerical standardized.

Here is an example of how the movie information looks like:

```python
{'adult': False,
 'collection': 210,
 'genres': [14, 1, 11],
 'original_language': 0,
 'popularity': 5.218749755002595,
 'production_companies': [96],
 'production_countries': [0],
 'release_year': 1995,
 'release_month': 10,
 'revenue': 4.893588591235185,
 'runtime': -0.5098445413830461,
 'spoken_languages': [0],
 'title': 'Toy Story',
 'vote_average': 1.2557064312220563,
 'vote_count': 1.8032194192281197,
 'budget': 1.1843770075921112,
 'revenue_d': 5.626649137875692}
```

## Getting started:

1. Download the static ml20m dataset and the movie embeddings
2. Clone this repo
3. Infos_pca128.pytorch (embeddings) into the RecNN/data folder
4. Run notes/3. DDPG and see the results

## TD3 results

Here you can see the training process of the network:

<p align="center"> 
<img src="./res/Losses.png">
</p>

Here is a pairwise similarity matrix of real (first image) and generated (second image) actions (movies)


<p align="center"> 
    <img src="./res/real_dist.png">
</p>

<p align="center"> 
    <img src="./res/gen_dist.png">
</p>

It doesn't seem to overfit much. Here you can see the Kernel Density Estimation for Autoencoder Reconstruction scores. I use it as an anomaly detection metric. (Wasserstein Distance = ~50)

<p align="center"> 
<img src="./res/Anomaly_Detection.png">
</p>

 # Downloads
- [Movie Embeddings](https://drive.google.com/open?id=1kTyu05ZmtP2MA33J5hWdX8OyUYEDW4iI)
- [Misc Data](https://drive.google.com/open?id=1TclEmCnZN_Xkl3TfUXL5ivPYmLnIjQSu)
- [Metadata for predictions](https://drive.google.com/open?id=1xjVI4uVQGsQ7tjOJ3594ZXmAEC_6yX0e)

## Models

- [Articles 1,2: DDPG, TD3, BCQ](https://drive.google.com/open?id=1a15mvtXZwOOSj9aQJNCxNlPMYREYYDxg)

## FAQ:

**What are the films ids?**
 
 It uses movies.csv from ML20M. The field is movieId
 
 **Something in the RL Losses looks weird**
 
It is fine for the RL losses. Keep in mind that RL algorithms utilize neural networks for calculating the loss functions (Policy Loss) or some wacky stuff like Temporal Difference bootstapping with target network for Value Loss.
 
 **What is the size of ...?**
 
| Name       | Dimensions  | Base Type |
|------------|----------------|-----------|
| State      | 1290           | float     | 
| Action     | 128            | float     | 
| Reward     | 1              | int8      | 
| Next_State | 1290           | float     | 
| Done       | 1              | bool      | 

P.S. all types are downcasted to float32 in the PyTorch backend.

## Medium Articles (WIP)
I wrote some medium articles explaining how this works: 

<p align="center"> 
   <a href="https://towardsdatascience.com/reinforcement-learning-ddpg-and-td3-for-news-recommendation-d3cddec26011">
        <img src="./res/Article.png">
    </a>
</p>

<p align="center"> 
   <a href="https://towardsdatascience.com/deep-reinforcement-learning-for-news-recommendation-part-1-architecture-5741b1a6ed56">
        <img src="./res/Article old.png">
    </a>
</p>



License
----

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.

