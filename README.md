# keras-tensorflow-exp-osx
This is a keras with tensorflow experiment on osx/mac-mini(late 2012)<BR>
<BR>
1. why keras ? <BR>
For me test a code shown bellow.<BR>
http://benjaminbolte.com/blog/2016/keras-language-modeling.html<BR>
<BR>
2. then ?<BR>
installed keras and tensorflow.<BR>
<BR>
3. run<BR>
I got this.<BR>
:(snip)<BR>
<pre>
8/8 [==============================] - 0s - loss: 8.0590<BR>
Traceback (most recent call last):<BR>
  File "emb_tst.py", line 67, in <module><BR>
    print('{}: {}'.format(idx2word[i], embeddings[i]))<BR>
  File "/Library/Python/2.7/site-packages/tensorflow/python/ops/variables.py", line 610, in <lambda><BR>
    setattr(Variable, operator, lambda a, b: Variable._RunOp(operator, a, b))<BR>
  File "/Library/Python/2.7/site-packages/tensorflow/python/ops/variables.py", line 625, in _RunOp<BR>
    return getattr(ops.Tensor, operator)(a._AsTensor(), b)<BR>
  File "/Library/Python/2.7/site-packages/tensorflow/python/ops/array_ops.py", line 167, in _SliceHelper<BR>
    sliced = slice(tensor, indices, sizes)<BR>
  File "/Library/Python/2.7/site-packages/tensorflow/python/ops/array_ops.py", line 217, in slice<BR>
    return gen_array_ops._slice(input_, begin, size, name=name)<BR>
  File "/Library/Python/2.7/site-packages/tensorflow/python/ops/gen_array_ops.py", line 1318, in _slice<BR>
    name=name)<BR>
  File "/Library/Python/2.7/site-packages/tensorflow/python/ops/op_def_library.py", line 655, in apply_op<BR>
    op_def=op_def)<BR>
  File "/Library/Python/2.7/site-packages/tensorflow/python/framework/ops.py", line 2156, in create_op<BR>
    set_shapes_for_outputs(ret)<BR>
  File "/Library/Python/2.7/site-packages/tensorflow/python/framework/ops.py", line 1612, in set_shapes_for_outputs<BR>
    shapes = shape_func(op)<BR>
  File "/Library/Python/2.7/site-packages/tensorflow/python/ops/array_ops.py", line 865, in _SliceShape<BR>
    input_shape.assert_has_rank(ndims)<BR>
  File "/Library/Python/2.7/site-packages/tensorflow/python/framework/tensor_shape.py", line 605, in assert_has_rank<BR>
    raise ValueError("Shape %s must have rank %d" % (self, rank))<BR>
  ValueError: Shape (8, 3) must have rank 1<BR>
</pre>
4. how to fix this?<BR>
  https://www.scipy.org/scipylib/building/macosx.html<BR>
  http://stackoverflow.com/questions/14821297/scipy-build-install-mac-osx<BR>
  https://github.com/fchollet/keras/issues/1303<BR>
  download scipy from url shown bellow.<BR>
  https://github.com/scipy/scipy<BR>
  download gfortran from bellow URL<BR>
  https://r.research.att.com/tools/<BR>
  gcc-4.2 (Apple build 5666.3) with GNU Fortran 4.2.4 for Mac OS X 10.7 (Lion):gcc-42-5666.3-darwin11.pkg <BR>
  install gcc-42-5666.3-darwin11.pkg<BR>
  export CC=gcc-4.2<BR>
  export CXX=g++-4.2<BR>
  export FFLAGS=-ff2c<BR>
  alias gfortran="/usr/bin/gfortran-4.2"<BR>
  ln -s /usr/bin/gfortran-4.2 /usr/local/bin/gfortran<BR>
  and do your misc preparation you need to build scipy.<BR>
  Then build it.<BR>
  python setup.py build<BR>
5. pip uninstall scipy<BR>
6. python setup.py install<BR>
7. edit emb_tst.py<BR>
   I got idea url shown bellow.<BR>
   http://qiita.com/supersaiakujin/items/b9c9da9497c2163d5a74<BR>
   bellow this line, add lines bellow.<BR>
   from keras.models import Model<BR>
   #here insert bellow.<BR>
   import tensorflow as tf<BR>
   import keras.backend.tensorflow_backend as KTF<BR>
   old_session = KTF.get_session() <BR>
   session = tf.Session('')<BR>
   KTF.set_session(session)<BR>
   session.as_default()<BR>
   and, change line bellow with comment.<BR>
   #embeddings = predict_green.layers[1].W.get_value()<BR>
   and, insert this line bellow.<BR>
   embeddings = predict_green.layers[1].W.eval(session)<BR>
<BR>
8. then execution log<BR>
<pre>
bash-3.2# python emb_tst.py
Using TensorFlow backend.
{'sarah': 0, 'sam': 1, 'hannah': 2, 'is': 3, 'green': 4, 'not': 5, 'bob': 6, 'red': 7}
[[1 3 7]
 [2 5 7]
 [2 3 4]
 [6 3 4]
 [6 5 7]
 [1 5 4]
 [0 3 7]
 [0 5 4]]
Epoch 1/5000
8/8 [==============================] - 0s - loss: 6.9579
Epoch 2/5000
8/8 [==============================] - 0s - loss: 3.8016
Epoch 3/5000
8/8 [==============================] - 0s - loss: 3.7670
Epoch 4/5000
8/8 [==============================] - 0s - loss: 3.7334
Epoch 5/5000
8/8 [==============================] - 0s - loss: 3.6996
Epoch 6/5000
8/8 [==============================] - 0s - loss: 3.6664
Epoch 7/5000
8/8 [==============================] - 0s - loss: 3.6333
Epoch 8/5000
8/8 [==============================] - 0s - loss: 3.6008
Epoch 9/5000
8/8 [==============================] - 0s - loss: 3.5684
Epoch 10/5000
8/8 [==============================] - 0s - loss: 3.5366
Epoch 11/5000
8/8 [==============================] - 0s - loss: 3.5049
Epoch 12/5000
8/8 [==============================] - 0s - loss: 3.4737
Epoch 13/5000
8/8 [==============================] - 0s - loss: 3.4427
Epoch 14/5000
8/8 [==============================] - 0s - loss: 3.4121
:(snip)
:(snip)
:(snip)
Epoch 4998/5000
8/8 [==============================] - 0s - loss: 0.0021
Epoch 4999/5000
8/8 [==============================] - 0s - loss: 0.0021
Epoch 5000/5000
8/8 [==============================] - 0s - loss: 0.0021
sarah: [ 0.54737252  0.11190682 -0.23395848]
sam: [ 0.57063431  0.05031192 -0.19117546]
hannah: [-1.45788646 -1.77522397 -2.33706784]
is: [ 0.1821208   0.03916844 -0.18679892]
green: [-0.2229352   0.00728517  0.30158323]
not: [ 0.78061891  1.44727206  2.27976871]
bob: [-0.50716257 -0.08670964  0.24690872]
red: [-0.88534838 -1.24907482 -1.79847264]
bash-3.2# 
</pre>

9. Consideration(s)<BR>
(1) RNN is on theano, so tensorflow should use SimpleRNN instead.<BR>
(2) I did not understand why 3d distribution of points differs original author now(sorry).<BR>
(3) why differs points distribution, further research(update in someday if i got).<BR>
(4) pip install scipy --upgrade --ignore-installed cause no change (3d points differ)<BR>
residue is RNN(teano) vs SimpleRNN... further reseach :-).<BR>
<BR>
10. Further Consideration(s)<BR>
To improve accuracy and cut off loss, tweaked source code.<BR>
Then, I got result shown bellow.<BR>
<BR>
Train on 6 samples, validate on 2 samples<BR>
Epoch 1/5500<BR>
6/6 [==============================] - 0s - loss: 7.5065 - val_loss: 0.5866<BR>
(snip)<BR>
Epoch 5500/5500<BR>
6/6 [==============================] - 0s - loss: 0.0418 - val_loss: 0.3357<BR>
sarah: [ 0.00385498  0.01372873 -0.0130481 ]<BR>
sam: [-0.97111976  0.33307493  0.57458359]<BR>
hannah: [ 1.12190282 -0.43887946 -0.69906741]<BR>
is: [-0.20908281  0.41405582  0.29519755]<BR>
green: [-0.20227642  0.4038054   0.31272194]<BR>
not: [-0.52407306  0.0273238   0.10946918]<BR>
bob: [ 1.45334864 -0.0892938  -0.44378492]<BR>
red: [-0.4351255   0.13918306  0.16001579]<BR>
circumstances:<BR>
(1) used Theano with keras.<BR>
(2) memory wiped with memory cleanning tool on osx.<BR>
(3) tested osx native env.<BR>
(4) used test.py on this repos.<BR>
(5) diff to original is on DIFF.txt.<BR>
(6) original goes wrong result on my environment, so tweak<BR>
<pre>
(7) python version<BR>
Python 2.7.9 (v2.7.9:648dcafa7e5f, Dec 10 2014, 10:10:46) <BR>
[GCC 4.2.1 (Apple Inc. build 5666) (dot 3)] on darwin<BR>
(8) keras version<BR>
>>> import keras as k<BR>
>>> k.__version__<BR>
'1.0.4'<BR>
(9) theano version<BR>
>>> import theano as t<BR>
>>> t.__version__<BR>
'0.8.2'<BR>
(10) numpy version<BR>
>>> import numpy as n<BR>
>>> n.__version__<BR>
'1.11.0'<BR>
(11) memory & cpu<BR>
CPU: 2.3ghz Intel Core i7<BR>
Memory: 4GB 1600mhz DDR3<BR>
other is normal Mac Mini(Late 2012)<BR>
</pre>
<BR>
11. For Research matters<BR>
(1) Embeddings is a layer, but initialization for this layer is not clear.<BR>
https://github.com/fchollet/keras/issues/2196<BR>
Model with one Layer fails #2196<BR>
If you run this code on above url, you will get warn shown bellow.<BR>
<pre>
UserWarning: Model inputs must come from a Keras Input layer, they cannot be the output of a previous non-Input layer. Here, <BR>
a tensor specified as input to "model_3" was not an Input tensor, it was generated by layer layer_1.<BR>
#Note that input tensors are instantiated via `tensor = Input(shape)`.<BR>
#The tensor that caused the issue was: input_1<BR>
#  str(x.name))<BR>
</pre>
(2) test.py and input_1 issue<BR>
under construction:-)<BR>
(3) important fact
i tested with several combinations.
(3-1) ubuntu on vbox on osx
(3-2) docker on osx(vbox)
but, result matrix were not similar with original one, so i decided tweak code and back to osx env.


