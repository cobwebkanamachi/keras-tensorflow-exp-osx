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
  download gfortran from bellow URL<BR>
  https://r.research.att.com/tools/<BR>
  gcc-4.2 (Apple build 5666.3) with GNU Fortran 4.2.4 for Mac OS X 10.7 (Lion):gcc-42-5666.3-darwin11.pkg <BR>
  install gcc-42-5666.3-darwin11.pkg<BR>
  export CC=gcc-4.2<BR>
  export CXX=g++-4.2<BR>
  export FFLAGS=-ff2c<BR>
  alias gfortran="/usr/bin/gfortran-4.2"<BR>
  ln -s /usr/bin/gfortran-4.2 /usr/local/bin/gfortran<BR>
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
