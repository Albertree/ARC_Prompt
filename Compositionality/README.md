# Compositionality
## Directory Structure
```
├─data: contains JSONL file for ARC tasks and TXT file for DSLs
|
├─model
|  ├─methods: contains a code for prompting techniques such as generation, vote, parsing, and regularization parser.
|  ├─prompts: contains a prompt code used when each prompting technique generates suggestions or evaluates value.
|  ├─tasks: contains codes that manage ARC tasks and their prompts.
|
├─result: contains results that consist of every step with selected DSLs
```


## Explanation about Python Codes
```compositionality_test_tool.py```: A GUI tool for experimenting with compositionality in humans.

```id_generator.py```: mapping ARC problem with corresponding task ID.

```obj_create.py```: makes arc.json from arc_no_object.json to contain the object information from ```PnP.py```.

```output_grid.py```: makes output grids according to the result of ```tot_arc_solver.py```.

```pnp.py```: detects the object for each ARC task's input grid using [PnP algorithm](https://openreview.net/forum?id=F9QfmL6IjZ).

```tot_arc_solver.py```: solves ARC tasks with ToT.

```utils.py```: contains functions for ```visualization.py```.

```visualization.py```: visualizes results.


## Quick Start
0. Check environment variables & move to the compositionality directory.
```
echo $AZURE_OPENAI_API_KEY
echo $AZURE_OPENAI_ENDPOINT
echo $AZURE_OPENAI_DEPLOYMENT_NAME

cd Compositionality
```
1. Detect objects in ARC tasks.
```
python obj_create.py
```
2. Run ARC solver.
```
python tot_arc_solver.py
```
3. Map the problem with Task ID.
```
python id_generator.py
```
4. Generate the expected output grid for each ARC task.
```
python output_grid.py
```
5. Visualize results.
```
python visualization.py
```
6. Check visualization results (HTML files) that are located in each subdirectory of the result directory.
```
xdg-open result/result_correct.html
xdg-open result/result_incorrect.html
```
7. Move to the origin directory.
```
cd ..
```

## Quick Start - GUI testing tool
If you want to the GUI tool for experimenting with compositionality in humans, then you can follow the steps below.

0. move to the compositionality directory.
```
cd Compositionality
```
1. Run python code
```
python compositionality_test_tool.py
```
2. Enter the user ID. This will be used later to record user logs in the CSV file.
3. Click the 'Main Test,' but if you want to practice before taking the main test, then click the 'Exercise Test'.

Detail about dsl button.
* ```Rotate Left```, ```Rotate Right```, ```Flip Vertical```, and ```Flip Horizontal``` transform the entire grid.
* ```X Line``` and ```Pixel Color``` perform their functions after selecting coordinates and choosing a color in the test output grid.
* ```Horizontal Line```, ```Vertical Line```, and ```Diagonal Line``` require selecting two coordinates to perform their functions. To select the two coordinates, you need to click the coordinates while holding down the Shift or Control key.
* ```X Line```, ```Horizontal Line```, ```Vertical Line```, and ```Diagonal Line``` do not color the selected coordinates.
* ```Move Right```, ```Move Left```, ```Move Up```, ```Move Down```, ```Rotate Right Obj```, ```Rotate Left Obj```, ```Flip Vertical Obj```, and ```Flip Horizontal Obj``` functions can be used by clicking on the object on the right side of the GUI screen and then selecting the desired function.
* The ```Object Color``` function colors an object with the selected color after you click on the object, choose a color, and then click the function button.
* Click ```Complete``` when you have finished solving the problem. This will move you to the next problem.
* You can use up to 10 DSLs per problem. ```Complete``` is also considered using a DSL. The number of used DSLs is displayed as "Step:" below the selected color. When the step count reaches 10, you will move to the next problem.

## What is Compositionality?
**Compositionality refers to the ability to generate complex linguistic expressions using simpler ones.** 
This characteristic allows individuals to effectively tackle more complex tasks by breaking sub-tasks down into simpler steps, supporting the notion that humans can solve more complex tasks when faced with them. 
Strong compositionality enables the resolution of complex tasks and facilitates transparent descriptions of the process, which is also an important aspect from the perspective of LLMs. 
This section tests compositionality by treating ARC tasks as stepwise compositions of simpler functions.


## How to Experiment to Evaluate Compositionality of LLM?
The experiment is conducted to determine the compositionality of the LLM using given DSLs.
DSLs are given by ```tot\data\prototype_arc\dsl.txt``` with python-coded.
DSLs and tasks are given by ```tot\prompts\arc.py```.
In ```tot\tasks\arc.py```, the ARCTask class makes LLM generate an answer and the ARCEnv class applies the chosen DSL to the current state and object.
You can add your API in ```tot\models.py```.

The below figures represent all of the processes of the experiment.

<img src="https://github.com/GIST-DSLab/ARC_Prompt/assets/22788924/9fd63bba-acaf-427b-9c20-6e2a4cb32d70"  width="100%" height="100%"/>
 Since LLMs correctly understand the innated rules in DSLs, LLM generates valid outputs with required actions such as coloring, rotating, drawing a line, and flipping when we give information about DSL, current state, and object.

</br>
</br>

<img src="https://github.com/GIST-DSLab/ARC_Prompt/assets/22788924/c3bf3d8b-91e4-4a43-b613-a3608aacbeac"  width="100%" height="100%"/>
 An example of the single step in an experimental process. 
 The LLM solver observes the current state and chooses the DSL from the list. Then, the LLM validator evaluates the selected DSLs by score. 
 Based on the score, the top two states are sent to the next step.


## Results
The accuracy is based on solving the problems in the training set, 260 questions with the same input/output dimensions, with ToT prompts and DSLs.
We also classify the tasks based on [ARC-Game](https://github.com/volotat/ARC-Game) and organize experimental results like below.

<div align="center">
  
|          | Entry | Easy  | Medium | Hard  | Etc   | Total |
|:--------:|:-----:|:-----:|:------:|:-----:|:-----:|:-----:|
| Tasks    | 5     | 152    | 65     | 10    | 28    | 260    |
| Correct  | 0     | 1     | 0      | 0     | 1     | 2     |
| Accuracy | 0.000% | 0.006% | 0.000%  | 0.000% | 0.036% | 0.008% |

</div>

## Etc
We modify [tanchongmin's code](https://github.com/tanchongmin/ARC-Challenge) to make the visualization code and use it to visualize the ARC grid. 
