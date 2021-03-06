Restricted Boltzmann Machine
===========
The initial RBM Contrastive Divergence algorithm implemented from <a href="http://blog.echen.me/2011/07/18/introduction-to-restricted-boltzmann-machines/" target="_blank">this blog.</a>

This version includes image encoding/decoding schemes, Contrastive Divergence training for a single RBM, deep, convoluted, and recurrent RBMs. Uses <a href="https://sites.google.com/site/piotrwendykier/software/parallelcolt" target="_blank">Parallel Colt</a> for matrix processing. Also includes a Multithreaded Deep RBM.

Results RBM(visual=6,hidden=4)
```
Training Data:
[[1.0, 1.0, 1.0, 0.0, 0.0, 0.0]
 [1.0, 0.0, 1.0, 0.0, 0.0, 0.0]
 [1.0, 1.0, 1.0, 0.0, 0.0, 0.0]
 [0.0, 0.0, 1.0, 1.0, 1.0, 0.0]
 [0.0, 0.0, 1.0, 1.0, 0.0, 0.0]
 [0.0, 0.0, 1.0, 1.0, 1.0, 0.0]]

Input:  [[0.0, 0.0, 0.0, 1.0, 1.0, 0.0]]
Output: [[0.0, 0.0, 1.0, 1.0, 1.0, 0.0]]

Inputs: [[0.0, 0.0, 0.0, 1.0, 1.0, 0.0] [0.0, 0.0, 1.0, 1.0, 0.0, 0.0]]
Outputs: [0.0, 0.0, 1.0, 1.0, 1.0, 0.0] [0.0, 0.0, 1.0, 1.0, 0.0, 0.0]]
```
Code for above Output:
```java
final RBM rbm = RBM_FACTORY.build(6, 3);
final ContrastiveDivergence contrastiveDivergence = new ContrastiveDivergence(new LearningParameters().setEpochs(25000));

contrastiveDivergence.learn(rbm, buildBetterSampleTrainingData());

// fetch two recommendations
final Matrix testData = DenseMatrix.make(new double[][]{{0, 0, 0, 1, 1, 0}, {0, 0, 1, 1, 0, 0}});
final Matrix hidden = contrastiveDivergence.runVisible(rbm, testData);
LOGGER.info(testData);
final Matrix visual = contrastiveDivergence.runHidden(rbm, hidden);
LOGGER.info(visual);
```        

Image Recognition

Shallow RBM - Input a 100x63 pixel image of a fighter jet at 24bit color resolution. Each RGB value is encoded as a 24 bit vector making a total input size of 100 x 24 x 63 bits.
<table>
   <tr>
      <td>Input<br/><img src="https://raw.githubusercontent.com/kennycason/rbm/master/src/main/resources/data/fighter_jet_small.jpg" width="200"/></td>
      <td>RBM Generated 24 bit<br/><img src="https://raw.githubusercontent.com/kennycason/rbm/master/src/main/resources/data/output/fighter_rendered_small_rendered_24bit.jpg" width="200"/></td>
      <td>RBM Generated 8 bit<br/><img src="https://raw.githubusercontent.com/kennycason/rbm/master/src/main/resources/data/output/fighter_rendered_small_8bit.jpg" width="200"/></td>
   </tr>
</table>

Deep RBM - Input a 400*250 pixel image of a fighter jet at 24bit color resolution. Each RGB value is encoded as a 24 bit vector making a total input size of 400 * 24 * 250 bits. That's 2.4 Million inputs to be learned.
<table>
   <tr>
      <td>Input<br/><img src="https://raw.githubusercontent.com/kennycason/rbm/master/src/main/resources/data/fighter_jet.jpg" width="200"/></td>
      <td>1 Epoch<br/><img src="https://raw.githubusercontent.com/kennycason/rbm/master/src/main/resources/data/output/fighter_rendered_large_24bit_deep_1_iteration.jpg" width="200"/></td>
      <td>11 Epochs<br/><img src="https://raw.githubusercontent.com/kennycason/rbm/master/src/main/resources/data/output/fighter_rendered_large_24bit_deep_11_iterations.jpg" width="200"/></td>
   </tr>
</table>

RBM - Learn 9 Pokemon Image (Full dataset contains 151 pokemon) 60x60 pixels, 24bit resolution.
<table>
   <tr>
      <td><img src="https://raw.githubusercontent.com/kennycason/rbm/master/src/main/resources/data/pokemon/output/9_single_rbm/pokemon_0.bmp" width="180"/></td> 
      <td><img src="https://raw.githubusercontent.com/kennycason/rbm/master/src/main/resources/data/pokemon/output/9_single_rbm/pokemon_1.bmp" width="180"/></td>
      <td><img src="https://raw.githubusercontent.com/kennycason/rbm/master/src/main/resources/data/pokemon/output/9_single_rbm/pokemon_2.bmp" width="180"/></td>
   </tr><tr>
      <td><img src="https://raw.githubusercontent.com/kennycason/rbm/master/src/main/resources/data/pokemon/output/9_single_rbm/pokemon_3.bmp" width="180"/></td>
      <td><img src="https://raw.githubusercontent.com/kennycason/rbm/master/src/main/resources/data/pokemon/output/9_single_rbm/pokemon_4.bmp" width="180"/></td>
      <td><img src="https://raw.githubusercontent.com/kennycason/rbm/master/src/main/resources/data/pokemon/output/9_single_rbm/pokemon_5.bmp" width="180"/></td>
   </tr><tr>
      <td><img src="https://raw.githubusercontent.com/kennycason/rbm/master/src/main/resources/data/pokemon/output/9_single_rbm/pokemon_6.bmp" width="180"/></td>
      <td><img src="https://raw.githubusercontent.com/kennycason/rbm/master/src/main/resources/data/pokemon/output/9_single_rbm/pokemon_7.bmp" width="180"/></td>
      <td><img src="https://raw.githubusercontent.com/kennycason/rbm/master/src/main/resources/data/pokemon/output/9_single_rbm/pokemon_8.bmp" width="180"/></td>
   </tr>
</table>

RBM - Note how having a white BG (max value input) negatively affects learning, where as a Black (zero value input) converges quickly. They were trained on identical RBMs for the same number of epochs. 
<table>
   <tr>
      <td><img src="https://raw.githubusercontent.com/kennycason/rbm/master/src/main/resources/data/pokemon/output/change_bg/pokemon_white_0.bmp" width="180"/></td> 
      <td><img src="https://raw.githubusercontent.com/kennycason/rbm/master/src/main/resources/data/pokemon/output/change_bg/pokemon_white_1.bmp" width="180"/></td>
      <td><img src="https://raw.githubusercontent.com/kennycason/rbm/master/src/main/resources/data/pokemon/output/change_bg/pokemon_white_2.bmp" width="180"/></td>
   </tr><tr>
      <td><img src="https://raw.githubusercontent.com/kennycason/rbm/master/src/main/resources/data/pokemon/output/change_bg/pokemon_black_0.bmp" width="180"/></td> 
      <td><img src="https://raw.githubusercontent.com/kennycason/rbm/master/src/main/resources/data/pokemon/output/change_bg/pokemon_black_1.bmp" width="180"/></td>
      <td><img src="https://raw.githubusercontent.com/kennycason/rbm/master/src/main/resources/data/pokemon/output/change_bg/pokemon_black_2.bmp" width="180"/></td>
   </tr>
</table>

Number Recognition
```
// INPUT
INFO  nn.rbm.TestRBM -
□□□□□□□□□□□□□□□□□□□□□□□□□□□□
□□□□□□□□□□□□□□□□□□□□□□□□□□□□
□□□□□□□□□□□□□□□□□□□□□□□□□□□□
□□□□□□□□□□□□□□□□□□□□□□□□□□□□
□□□□□□□□□□□□□□□□□□□□□□□□□□□□
□□□□□□□□□□□□□□□□□■■□■■■□□□□□
□□□□□□□□□□□■■■■■■■■■■■■□□□□□
□□□□□□□□■■■■■■■■■■□□□□□□□□□□
□□□□□□□□■■■■■■■■■■□□□□□□□□□□
□□□□□□□□□■□■■■□□□■□□□□□□□□□□
□□□□□□□□□□□■■□□□□□□□□□□□□□□□
□□□□□□□□□□□■■■□□□□□□□□□□□□□□
□□□□□□□□□□□□■■□□□□□□□□□□□□□□
□□□□□□□□□□□□□■■■□□□□□□□□□□□□
□□□□□□□□□□□□□□■■■□□□□□□□□□□□
□□□□□□□□□□□□□□□■■■■□□□□□□□□□
□□□□□□□□□□□□□□□□□■■■□□□□□□□□
□□□□□□□□□□□□□□□□□■■■□□□□□□□□
□□□□□□□□□□□□□□□■■■■■□□□□□□□□
□□□□□□□□□□□□□■■■■■■■□□□□□□□□
□□□□□□□□□□□□■■■■■■□□□□□□□□□□
□□□□□□□□□□■■■■■■□□□□□□□□□□□□
□□□□□□□■■■■■■■□□□□□□□□□□□□□□
□□□□□■■■■■■■■□□□□□□□□□□□□□□□
□□□□■■■■■■■□□□□□□□□□□□□□□□□□
□□□□□□□□□□□□□□□□□□□□□□□□□□□□
□□□□□□□□□□□□□□□□□□□□□□□□□□□□
□□□□□□□□□□□□□□□□□□□□□□□□□□□□

INFO  nn.rbm.TestOldRBM -
□□□□□□□□□□□□□□□□□□□□□□□□□□□□
□□□□□□□□□□□□□□□□□□□□□□□□□□□□
□□□□□□□□□□□□□□□□□□□□□□□□□□□□
□□□□□□□□□□□□□□□□□□□□□□□□□□□□
□□□□□□□□□□□□□□□□□□□□□□□□□□□□
□□□□□□□□□□□□□□□□■■■□□□□□□□□□
□□□□□□□□□□□□□□□■■■■■□□□□□□□□
□□□□□□□□□□□□□□□■■■■■□□□□□□□□
□□□□□□□□□□□□□■■■■■■■□□□□□□□□
□□□□□□□□□□□□■■■■■■■□□□□□□□□□
□□□□□□□□□□□□■■□□■■■□□□□□□□□□
□□□□□□□□□□□■■□□■■■□□□□□□□□□□
□□□□□□□□□□□■■□□■■□□□□□□□□□□□
□□□□□□□□□□□■■□■■□□□□□□□□□□□□
□□□□□□□□□□□□■■■■□□□□□□□□□□□□
□□□□□□□□□□□□■■■■□□□□□□□□□□□□
□□□□□□□□□□□□■■■■□□□□□□□□□□□□
□□□□□□□□□□□□■■□■■□□□□□□□□□□□
□□□□□□□□□□□■■□□■■□□□□□□□□□□□
□□□□□□□□□□□■■□□■■□□□□□□□□□□□
□□□□□□□□□□■■□□□■■□□□□□□□□□□□
□□□□□□□□□□■■□□□■■□□□□□□□□□□□
□□□□□□□□□□■■□□■■□□□□□□□□□□□□
□□□□□□□□□□■■■■■■□□□□□□□□□□□□
□□□□□□□□□□□■■■■□□□□□□□□□□□□□
□□□□□□□□□□□□□□□□□□□□□□□□□□□□
□□□□□□□□□□□□□□□□□□□□□□□□□□□□
□□□□□□□□□□□□□□□□□□□□□□□□□□□□

...

INFO  nn.rbm.learn.OldContrastiveDivergence - Start Learning (7 samples)
INFO  nn.rbm.learn.OldContrastiveDivergence - Epoch: 0/15000, error: 1305.5197925558577, time: 0.059s
INFO  nn.rbm.learn.OldContrastiveDivergence - Epoch: 100/15000, error: 57.300594478427854, time: 0.004s
INFO  nn.rbm.learn.OldContrastiveDivergence - Epoch: 200/15000, error: 15.952329441261893, time: 0.003s
INFO  nn.rbm.learn.OldContrastiveDivergence - Epoch: 300/15000, error: 5.4044291068371155, time: 0.003s
INFO  nn.rbm.learn.OldContrastiveDivergence - Epoch: 400/15000, error: 2.602268788842556, time: 0.003s
INFO  nn.rbm.learn.OldContrastiveDivergence - Epoch: 500/15000, error: 1.4970038901297982, time: 0.003s
INFO  nn.rbm.learn.OldContrastiveDivergence - Epoch: 600/15000, error: 1.1067551950980756, time: 0.003s
INFO  nn.rbm.learn.OldContrastiveDivergence - Epoch: 700/15000, error: 0.8295110168889177, time: 0.003s
...
INFO  nn.rbm.learn.OldContrastiveDivergence - Epoch: 14400/15000, error: 0.002183322948934887, time: 0.003s
INFO  nn.rbm.learn.OldContrastiveDivergence - Epoch: 14500/15000, error: 0.0018464431984471126, time: 0.003s
INFO  nn.rbm.learn.OldContrastiveDivergence - Epoch: 14600/15000, error: 0.002316604784920346, time: 0.003s
INFO  nn.rbm.learn.OldContrastiveDivergence - Epoch: 14700/15000, error: 0.015824371649477142, time: 0.003s
INFO  nn.rbm.learn.OldContrastiveDivergence - Epoch: 14800/15000, error: 0.0033692543108419077, time: 0.003s
INFO  nn.rbm.learn.OldContrastiveDivergence - Epoch: 14900/15000, error: 0.006265503532066407, time: 0.003s
INFO  nn.rbm.TestOldRBM - Data Index: 0
INFO  nn.rbm.TestOldRBM -

INFO  nn.rbm.TestOldRBM -
□□□□□□□□□□□□□□□□□□□□□□□□□□□□
□□□□□□□□□□□□□□□□□□□□□□□□□□□□
□□□□□□□□□□□□□□□□□□□□□□□□□□□□
□□□□□□□□□□□□□□□□□□□□□□□□□□□□
□□□□□□□□□□□□□□□□□□□□□□□□□□□□
□□□□□□□□□□□□□□□■■■■□■■■■□□□□
□□□□□□□□□□□■■■■■■■■■■■■□□□□□
□□□□□□□□■■■■■■■■■■□□■□■□□□□□
□□□□□□□■■■■■■■■■■■□□□□□□□□□□
□□□□□□□□□□□■■■□□■■□□□□□□□□□□
□□□□□□□□□□□□■□□□□□□□□□□□□□□□
□□□□□□□□□□□■■■□□□□□□□□□□□□□□
□□□□□□□□□□□□■■□□□□□□□□□□□□□□
□□□□□□□□□□□□□■■□□□□□□□□□□□□□
□□□□□□□□□□□□□□■■■■□□□□□□□□□□
□□□□□□□□□□■□□□□■■■■□□□□□□□□□
□□□□□□□□□□□□□□□□□■■□□□□□□□□□
□□□□□□□□□□□□□□□□□■■■□□□□□□□□
□□□□□□□□□□□□□□□■■■■■□□□□□□□□
□□□□□□□□□□□□□■■■■■■□□□□□□□□□
□□□□□□□□□□■■■■■■■■□□□□□□□□□□
□□□□□□□□□□■■■■■□□□□□□□□□□□□□
□□□□□□□■■■■■■■□□□□□□□□□□□□□□
□□□□■■□■■■■■□□□□□□□□□□□□□□□□
□□□□□■■■■□■□□□□□□□□□□□□□□□□□
□□□□□□□□□□□□□□□□□□□□□□□□□□□□
□□□□□□□□□□□□□□□□□□□□□□□□□□□□
□□□□□□□□□□□□□□□□□□□□□□□□□□□□

INFO  nn.rbm.TestOldRBM -
□□□□□□□□□□□□□□□□□□□□□□□□□□□□
□□□□□□□□□□□□□□□□□□□□□□□□□□□□
□□□□□□□□□□□□□□□□□□□□□□□□□□□□
□□□□□□□□□□□□□□□□□□□□□□□□□□□□
□□□□□□□□□□□□□□□□□□□□□□□□□□□□
□□□□□□□□□□□□□□□□□■■□□□□□□□□□
□□□□□□□□□□□□□□□■■■■□□□□□□□□□
□□□□□□□□□□□□□□□■■■■■□□□□□□□□
□□□□□□□□□□□□□■■■■■■□□□□□□□□□
□□□□□□□□□□□□■■■■■■■■□□□□□□□□
□□□□□□□□□□□■■■■□■■■□□□□□□□□□
□□□□□□□□□□□■■□□□■□□□□□□□□□□□
□□□□□□□□□□□■■□□■■□□□□□□□□□□□
□□□□□□□□□□□■■□■■□□□□□□□□□□□□
□□□□□□□□□□□□■■■■□□□□□□□□□□□□
□□□□□□□□□□□□■■■□□□□□□□□□□□□□
□□□□□□□□□□□□■■□■■□□□□□□□□□□□
□□□□□□□□□□□□■□□■■□□□□□□□□□□□
□□□□□□□□□□□■■□□■■■□□□□□□□□□□
□□□□□□□□□□□■■□□■■□□□□□□□□□□□
□□□□□□□□□□■■□□□■■■□□□□□□□□□□
□□□□□□□□□□■□□□□■■□□□□□□□□□□□
□□□□□□□□□□■■□□■■■□□□□□□□□□□□
□□□□□□□□□□■■■■■■□□□□□□□□□□□□
□□□□□□□□□□□□■■□□□□□□□□□□□□□□
□□□□□□□□□□□□□□□□□□□□□□□□□□□□
□□□□□□□□□□□□□□□□□□□□□□□□□□□□
□□□□□□□□□□□□□□□□□□□□□□□□□□□□

...
```
