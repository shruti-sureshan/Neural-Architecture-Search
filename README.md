# Neural-Architecture-Search

**Problem Statement:**<br/>
Your task is to create a search algorithm to search in a Neural Architecture Search (NAS) space for the best performing Convolutional Neural Network (CNN) architecture on the fashion-mnist data-set. Best performance is defined as having a test accuracy above 75% and the lowest parameter count model your algorithm can find. You will encounter a trade-off between test accuracy and parameter count. Your search algorithm should balance this trade-off and you should be able to justify your choices regarding this balancing.<br/><br/>
Since the space of all possible neural networks is very large, there are some constraints on the CNN architecture that your search function should return.<br/><br/>
Your search algorithm should search for models with the following basic structure. Sequential model (no skip connections) having the following layers,<br/>
1. Any number of normal(NC) CNN layers
   - torch.nn.Conv2d or tf.keras.layers.Conv2D
   - A normal CNN layer has the following constraints,
     - stride=1
     - padding=same
     - 1 <= kernel size < 8
     - Any of the following activation functions,
       - relu
       - sigmoid
       - tanh
       - swish
       - gelu
2. Exactly 2 reduction(RC) CNN layers
   - torch.nn.Conv2d or tf.keras.layers.Conv2D
   - A reduction layer has the same constraints as the normal layer except the following,
     - stride=2
     - padding=valid
3. Exactly the following structure as the final layers,
   - First, a Global Average Pooling 2D layer.
     - torch.nn.AvgPool2d(kernel size=layer input image size) or tf.keras.layers.GlobalAveragePooling2D
   - Then, a Fully Connected/Dense layer with 64 units (such that output is shape (batch size, 10)).
     - This layer can have any of the allowed activations in NC layer but all final layers must use same activation.
     - torch.nn.Linear(64) or tf.keras.layers.Dense(64)
   - Then, a Fully Connected/Dense layer with 10 units (such that output is shape (batch size, 10))
     - This layer can have any of the allowed activations in NC layer but all final layers must use same activation.
     - torch.nn.Linear(10) or tf.keras.layers.Dense(10)
 
 The normal(NC) and reduction(RC) layers can be in any order as long as the constraints above are maintained. The final layers of you model should be exactly as mentioned in these constraints. All of the above constraints apply only to your final output. <br/>
 
Once your algorithm finds the best architecture, it should represent the architecture as a genome string (explained below) which will be fed to the training function given below to get the final accuracy.<br/><br/>


**Genome String Format:**<br/>
A model genome is a string having ; separated parts where each part describes a specific layer. For example, this cell genome has 1 normal convolution layer followed by 2 reduction convolution layers and relu activation in the final layer.<br/>
NC 10 2 sigmoid;RC 10 3 relu;RC 10 3 relu;FL relu;<br/><br/>
In the above example the normal layer has 10 CNN filters having a kernel size of 2 and sigmoid activation. After the normal layer it has 2 reduction layers with 10 filters, kernel size of 3 and relu activation function. This example therefore has 3 internal layers. Finally, the genome specifies relu activation for the final layers(FL).<br/><br/>
The format for each genome part for NC and RC layers is as follows,<br/>
\<NC or RC\> <no. of CNN filters> <kernel size LESS THAN 8> \<activation function\>; <br/>
  
After all NC and RC genome parts the final layer genome part has the following format, <br/>
FL \<activation function\>; <br/>
  
With the above, the full genome format is, <br/>
<any number of FC/RC genome parts>;FL \<activation function\>;
  
Here,
- NC is normal genome and RC is reduction genome. Normal genomes keep the dimensions of the input image intact (i.e. stride=1), while reduction genomes make the output image dimension half of the input dimension.
- Every genome MUST have EXACTLY 2 RC parts. These RC parts can be placed anywhere in the genome.
