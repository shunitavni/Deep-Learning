# Deep-Learning

In this project I developed an algorithm that predicts the winner of a given Tennis match.

In order to do that, I will be working on two networks:

1. Embedding network - trained to predict a single outcome, given two players, in order to learn how to represent a player using a vector.
2. Prediction network - given two tennis player, this network predicts who is about to win. Each player is represented by a vector that is learned by the embedding network.




**Dataset**

The dataset is composed of a collection of Tennis matches and their outcomes, provided in multiple files. You should download the data from https://drive.google.com/open?id=1ie6WSl8qkknpSGAFt32Gj7Tf0JAhTLu0 and place the folder somewhere in your Google drive. We provide you the following function *read_data_for_embedding_network*, which reads the files and create a few dictionaries that will help you handle the data later, and a list of instances for training the embedding network. Here is a detailed description of the output of this function:

1. player_names - this is a dictionary that maps between a player id and a player name. This player id is unique, and it is given by the dataset owners.
2. player_id2index - this is a dictionary that maps between a player id and an index (starting at 0). This index is unique, and it is created by us. It will help us represent the players with a one-hot vector later.
3. players_n - an integer representing the number of players we have in the data (this will be the length of a one-hot vector representing a player).
4. embedding_data - the dataset to train the embedding network. This dataset is a simple list of lists. Each internal list represents an individual training instance, coomposed of three components: the winner index, the loser index and a binary represnting who scored more aces (a serve that was not responded) during the match.


**Embedding network**

Here I code the embedding network. An embedding network in general, is created for learning how to represent an input provided as a one-hot vector. It usually "projects" the one-hot vectors on another vector space, which has less dimensions that capture some meaningful information about the inputs. In this case, we will feed the network with the instances we got from the previous function: one hot vector to represent the winner, and ont hot vector to represent the loser. The network will try to predict who scored more aces. I  implement the network, based on the architecture depicted in the followin diagram. **Note - use ReLU activation, only after Linear 1.**


![alt text](https://docs.google.com/drawings/d/e/2PACX-1vRp5PeqjF_W-gQmpjcY32hx4n5kWKsySdvLr3avz-bc0kodQbSlvNFuTBX6lcLzS6eS1nwjW0IdVCTY/pub?w=960&h=720)


**Extracting embeddings**

In this section, I extracted the player representation from the embedding network. In fact, we don't realy care about the quality of prediction of the embedding network. We care more about the vector representation it learned for every player. The representation is in fact the weight matrix of the two first linear layers that created for each player of a given player pair. Each such weight matrix is of size (hidden_size X number_of_players). You can get that matrix of each of the two Linear layers, by calling their *parameters* function. This function returns a Python generator, and the first element is your matrix. Note that since you have two such layers, you will have two matrices; you are requested to add the two matrices together and divide by 2, and then you get a new matrix which will represent the players. This matrix, say M, will be of shape (hidden_size X number_of_players), so **M[:, 3]** for example, is a hidden_size vector representing player of index 3.



**Prediction network**

Now, we are ready to implement a simple network that predicts who wins a match, given two players and some additional match-leavel features. Please implement PredictionNetwork based on the following diagram. Each player is provided with it's embedding from the player_embedding matrix you calculated above. **Note - use ReLU after every Linear layer (except the last one of course).**

![alt text](https://docs.google.com/drawings/d/e/2PACX-1vTCAlmaool-UQ9be8wSdjjgOq0VaCiYsXwsHw0HDxZRXTkp6vBm08Ma8sQha-cKDjjeZhfzY2qT8if8/pub?w=960&h=720)
