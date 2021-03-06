
.. _training:

*********************
Training from scratch
*********************

Some Flash tasks have been pretrained on large data sets. To accelerate your training, calling the :func:`~flash.core.trainer.Trainer.finetune` method using a pretrained backbone will fine-tune the backbone to generate a model customized to your data set and desired task. If you want to train the task from scratch instead, pass `pretrained=False` parameter when creating your task. Then, use the :func:`~flash.core.trainer.Trainer.fit` method to train your model.


.. code-block:: python

    import flash
    from flash import download_data
    from flash.vision import ImageClassificationData, ImageClassifier

    # 1. download and organize the data
    download_data("https://download.pytorch.org/tutorial/hymenoptera_data.zip", 'data/')

    data = ImageClassificationData.from_folders(
        train_folder="data/hymenoptera_data/train/",
        valid_folder="data/hymenoptera_data/val/"
    )

    # 2. build the task, and turn off pre-training
    task = ImageClassifier(num_classes=2, pretrained=False)

    # 3. train!
    trainer = flash.Trainer()
    trainer.fit(model, data)


Training options
================

Flash tasks supports many advanced training functionalities out-of-the-box, such as:

* limit number of epochs

.. code-block:: python

    # train for 10 epochs
    flash.Trainer(max_epochs=10)

* Training on GPUs

.. code-block:: python

    # train on 1 GPU
    flash.Trainer(gpus=1)
    
* Training on multiple GPUs

.. code-block:: python

    # train on multiple GPUs
    flash.Trainer(gpus=4)

.. code-block:: python

    # train on gpu 1, 3, 5 (3 gpus total)
    flash.Trainer(gpus=[1, 3, 5])
    
* Using mixed precision training

.. code-block:: python

    # Multi GPU with mixed precision
    flash.Trainer(gpus=2, precision=16)

* Training on TPUs

.. code-block:: python

    # Train on TPUs
    flash.Trainer(tpu_cores=8)

You can add to the flash Trainer any argument from the Lightning trainer! Learn more about the Lightning Trainer `here <https://pytorch-lightning.readthedocs.io/en/stable/trainer.html>`_.


Trainer API
-----------

.. autoclass:: flash.core.trainer.Trainer
    :members:
    :exclude-members: training_step, validation_step, test_step, configure_optimizers, forward
