# Tensorflow Serving

Tensorflow serving was built to handle it on a server. It handles all the infrastructure such as using it, versioning it, maintaining the lifecycle. Dealing with with which model to use. taking the output from one model and passing it into another. Tensorflow is used by google in a production environment.

The basic architecture is that we have a file system that stores the models. We'll specify which version of the model we want to use. It will then serve that model and we will be able to send a request to it form the client to receive a classification.
- Tensorflow serving is meant for inference. Inference is managing models, giving versioned access, reference-counted lookup.
it can serve multiple models simultaneously.
- Can serve multiple versions of the same model.
- Written in C++
