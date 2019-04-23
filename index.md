# Overview

TF Encrypted is a Python library built on top of [TensorFlow](https://www.tensorflow.org) for researchers and practitioners to experiment with privacy-preserving machine learning. It provides an interface similar to that of TensorFlow, and aims at making the technology readily available without first becoming an expert in machine learning, cryptography, distributed systems, and high performance computing.

In particular, the library focuses on:

- **Usability**: The API and its underlying design philosophy make it easy to get started, use, and integrate privacy-preserving technology into pre-existing machine learning processes.
- **Extensibility**: The architecture supports and encourages experimentation and benchmarking of new cryptographic protocols and machine learning algorithms.
- **Performance**: Optimizing for tensor-based applications and relying on TensorFlow's backend means runtime performance comparable to that of specialized stand-alone frameworks.
- **Community**: With a primary goal of pushing the technology forward the project encourages collaboration and open source over proprietary and closed solutions.
- **Security**: Cryptographic protocols are evaluated against strong notions of security and [known limitations](#known-limitations) are highlighted.

# Installation

TF Encrypted is available as a package on [PyPI](https://pypi.org/project/tf-encrypted/) supporting Python 3.5+ and TensorFlow 1.12.0+ which can be installed using:

```bash
pip3 install tf-encrypted
```

# Usage

The following is an example of simple matmul on encrypted data using TF Encrypted:

```python
import tensorflow as tf
import tf_encrypted as tfe

def provide_input():
    # normal TensorFlow operations can be run locally
    # as part of defining a private input, in this
    # case on the machine of the input provider
    return tf.ones(shape=(5, 10))

# define inputs
w = tfe.define_private_variable(tf.ones(shape=(10,10)))
x = tfe.define_private_input('input-provider', provide_input)

# define computation
y = tfe.matmul(x, w)

with tfe.Session() as sess:
    # initialize variables
    sess.run(tfe.global_variables_initializer())
    # reveal result
    result = sess.run(y.reveal())
```

# Examples

See [Examples](https://github.com/tf-encrypted/tf-encrypted/examples).

# Background & Further Reading

The following texts provide further in-depth presentations of the project:

- [Secure Computations as Dataflow Programs](https://mortendahl.github.io/2018/03/01/secure-computation-as-dataflow-programs/) describes the initial motivation and implementation
- [Private Machine Learning in TensorFlow using Secure Computation](https://arxiv.org/abs/1810.08130) further elaborates on the benefits of the approach, outlines the adaptation of a secure computation protocol, and reports on concrete performance numbers
- [Experimenting with TF Encrypted](https://medium.com/dropoutlabs/experimenting-with-tf-encrypted-fe37977ff03c) walks through a simple example of turning an existing TensorFlow prediction model private

