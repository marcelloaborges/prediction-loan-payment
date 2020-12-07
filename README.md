# Puzzle


### Observations:
- To run the project just execute the <b>main.py</b> file like this if you have all the requeriments installed.
  - <b>python main.py</b>
- If you receive any message about a missing module, try installing it via <b>pip</b> or mail me at <b>marcelloaborges@gmail.com</b> for details.
- The project has already a trained <b>checkpoint</b> file that can be used if you want to avoid the training. The tester uses the <b>checkpoint</b>, so if you remove or rename it, run the training again before executing the test.
- The execution of the <b>main</b> will replace the <b>checkpoint<b> file and the <b>predictions<b> file.


### Requeriments:
- python: 3.6
- numpy: 1.11.0
- torch: 0.4.1
- pandas
- scipy
- ipykernel


## The problem:
As a credit company, it is important to know beforehand who is able to pay their loans and who
is not. The goal of this puzzle is to build a statistical/machine learning model to figure out which
clients are able to honor their debt.


## The goal:
Predict the probability of <b>default</b>, which is identified by the <b>default</b> variable in
the training dataset.


## The solution:
As this is a classification problem with one answer, yes or no, I decided to use a binary classification structure for the neural model with a sigmoid activation function in the output.<br />
After deciding the neural model, the first step was to analyse the data and identify the columns that could be removed due to its nonnumeric or non categorical values.<br />
So, I moved to the cleaning data phase. For the missing data, categorical columns were filled with default values and for the numerical columns, I used the median. <br />
With this scenery, after applying the default data normalization, I got <b>84%</b> of accuracy into a confusion matrix evaluation with the neural model structure described below.<br />
Then I tried to improve the results using regression for the missing data on the numerical columns. The loss established a little but the accuracy kept the same.<br />
The last improvement was removing the outliers and here there is a curious fact for this database. I used the quantile approach to generate NaN for the outliers and then remove them from the data, but I found that all the rows had at least one column with a NaN value generated after the quantile apply. Remove the rows with NaN generated by the quantile wasn't an option anymore, so I tried to use the mean instead of removing the rows with NaN values generated by the quantile but the results got worse, so I removed the outliers treatment and kept the solution as it was before this last step.


## Results:
- Loss: ~40% (binary cross-entropy)
- Accuracy: ~84% (round of the sigmoid output compared to the <b>default</b> column)


### The hyperparameters:
- The file with the hyperparameters configuration is the <b>main.py</b>.
- Configuration:
  - FEED_SIZE = 14
  - FC1_UNITS = 64 
  - FC2_UNITS = 32 
  - OUTPUT_SIZE = 1
  - LR = 1e-3
  - BATCH_SIZE = 512
  - EPOCHS = 10

- The neural model configuration is into the <b>model.py</b> file.
- The actual architecture is:    
  - Hidden: (feed_size, fc1_units)   - ReLU    
  - Dropout: fixed with 0.2
  - Hidden: (fc1_units, fc2_units)   - ReLU    
  - Output: (fc2, output_size)       - Sigmoid