# Train Utility

## Example

### Create ModelInfo

```ModelInfo``` store all the information about the model and training. The primary usage of ```ModelInfo``` is to record all the object and data that will be used in training so we can just pass this single object instead of many parameters into training function. </br>
Below will demonstrate creating ModelInfo and filling the minimum data into the ModelInfo.

```python
# create TrainProcess
info = ModelInfo()

# fill data
info.model      = # model (nn.Module)
info.optimizer  = # optimizer (optim)

info.epoch          = 100
info.batch_size     = 2
info.learning_rate  = 1e-4
```

### Create TrainProcess and Signaling

TrainProcess will be executed when a specific stage is reached. ```TrainProcess``` is the abstract class and should not be created directly. Below is the list of some children class of TrainProcess.

- DictLoad
- DictSave
- Folder
- Hook
- PythonFile
- ResultGraph
- ResultRecord

Example will use ```TrainProcess_Folder```, which will create a folder and normally needed to be called after the training is finished and some training result need a space to store.

```python
# create TrainProcess_Folder
process = TrainProcess_Folder()

# TrainProcess_Folder will use the data in ModelInfo to create the folder
# here, it is assumed that ModelInfo (model) is created and minimum data is filled
# we need to further fill more information for TrainProcess_Folder
info.save_path      = "./Result"
info.save_folder    = "Result_xxx"
```

To trigger the process, we need to call the ```ModelInfo.executeProcess(stage, info)```. There are some default stages in ```ModelInfo.Stage```.

- TRAIN_START
- ITERATION_TRAIN_START
- ITERATION_TRAIN_END
- ITERATION_VAL_START
- ITERATION_VAL_END
- TRAIN_END

Here we need to trigger ```TrainProcess_Folder``` at ```TRAIN_END``` stage.

```python
# data: Dict
# what inside data is depended on different stage and user is free to add more into it
info.executeProcess(ModelInfo.Stage.TRAIN_END, data)
```
