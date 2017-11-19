# 💊 CapsuleNet 💊

A PyTorch implementation of CapsuleNet as described in ["Dynamic Routing Between Capsules"](https://arxiv.org/abs/1710.09829) by Hinton et al.

## Run

```
▶ python train.py --help
usage: CapsNet [-h] [--epochs EPOCHS] [--data_path DATA_PATH]
               [--batch_size BATCH_SIZE] [--use_gpu] [--lr LR]
               [--log_interval LOG_INTERVAL] [--visdom] [--dataset DATASET]
               [--load_checkpoint LOAD_CHECKPOINT]
               [--checkpoint_interval CHECKPOINT_INTERVAL]
               [--checkpoint_dir CHECKPOINT_DIR] [--gen_dir GEN_DIR]

Example of CapsNet

optional arguments:
  -h, --help            show this help message and exit
  --epochs EPOCHS
  --data_path DATA_PATH
  --batch_size BATCH_SIZE
  --use_gpu
  --lr LR               ADAM learning rate (0.01)
  --log_interval LOG_INTERVAL
                        number of batches between logging
  --visdom              Whether or not to use visdom for plotting progrss
  --dataset DATASET     The dataset to train on, currently supported: MNIST,
                        Fashion MNIST
  --load_checkpoint LOAD_CHECKPOINT
                        path to load a previously trained model from
  --checkpoint_interval CHECKPOINT_INTERVAL
                        path to load a previously trained model from
  --checkpoint_dir CHECKPOINT_DIR
                        dir to store checkpoints in
  --gen_dir GEN_DIR     folder to store generated images in
(ml)
```

```
python train.py --visdom --checkpoint_interval=1 --epochs=10
```


## Results

| Dataset       | Epochs | Test loss | Test accuracy |
| ------------- | ------ |---------- | ------------- |
| MNIST         | 10     | 0.04356   | 98.803        |
| Fashion MNIST | 10     | 0.19429   | 86.580        |
| MNIST         | 50     | 0.03029   | 99.011        |


Using:

```
Conv layer:
- input channels: 1
- output channels: 256
- stride: 1
- kernel size: 9x9

Capsule layer 1:
- 8 capsules of size 1152
- input channels: 256
- output channels: 32

Capsule layer 2:
- 10 capsules of size 16 (10 classes in mnist)
- input channels: 32
- 3 iterations of the routing algorithm
```

### Generated images

I have yet to be able to reproduce the sharpness of reproduced images from the paper,
I suspect it the reason is because I am decoupling the digit cap results from so
that loss from the image generation is not backproped into capsnet. Another possibility
is that I need to use a decaying learning rate like in the paper.

Results of the decoder:

![batch](./imgs/mnist_generated_50_epoch.png)

Comparison of original and generated:

![compare](./imgs/generated_compare.png)

Results of training for 10 epochs on MNIST:

![train loss](./imgs/mnist_train_loss_10.png)
![train acc](./imgs/mnist_train_acc_10.png)
![Test Loss](./imgs/mnist_test_loss_10.png)
![train acc](./imgs/mnist_test_acc_10.png)

Results of training for 10 epochs on Fashion MNIST:

![train loss](./imgs/fmnist_train_loss_10.png)
![train acc](./imgs/fmnist_train_acc_10.png)
![Test Loss](./imgs/fmnist_test_loss_10.png)
![train acc](./imgs/fmnist_test_acc_10.png)


## References

I found these implementations useful when I got stuck

- https://github.com/llSourcell/capsule_networks
- https://github.com/cedrickchee/capsule-net-pytorch
