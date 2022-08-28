# Data100 Revision Notes

## Sampling
**Probability Sample:** chance of each individual selected is specified.
**Simple Random Sample:** uniformly at random without replacement

## Pandas

**Accessors**:

    df[colname/slices], 
    df.loc[rowname, colname], 
    df.iloc[rowpos, colpos]

**Groupby**: 

groupby a certain criteria to form sub dataframes, then aggregate based on the **agg funcs** like sum, count, mean, first/last, max.min, etc. Can be **filtered** by a certain function, and will elimnate a whole subdataframe if not met

**Pivot**: 

break into different groups by rows and also column, summing up (or custom agg func), then putting the result into a new df.

 **String operators**: 
  
    series.str.len(),
    series.str.lower()/upper(),
    ser.str.replace(pattern, replacement),
    series.str.contains(pattern),
    sereis.str.extract(pattern)

## Regex

**Quantifiers**:

    * // 0 or more occurrences
    + // 1 or more occurrences
    ? // 0 or 1 occurrences
    {x,y} // inclusively between x and y copies

**Metacharacters**: 

    [] // a set of equivalent single characters
    [^d] // not d
    A | BCD // matches either A or BCD
    () // grouping
    (?:) //grouping then ungrouping
    ^ // start anchor of the string
    $ // end anchor of the string

**Predefined characters**:

    . // matches any char not '\n'
    \w //matches a letter, digit or underscore
    \s //matches a single whitespace
    \d //decimal digit
    \ //escapes a special character

**Useful functions**: 

    re.split{pattern, string}
    re.sub{pattern, replace, string}
    re.findall{pattern, string} // returns a list of all matches

## Data Cleaning EDA

**Porperties**:
* Granularity: are they summaries (coarse) or are they individual record (fine)
* Scope: covering area of interest
* Temporality: when was the data last updated
* Faithfulness: how trustworthy?
  
**Missing Data & Defaults**:
* drop records with missing values, might induce bias
* imputation: by averaging or hot deck (random value)

## Linear Regression

**Simple**:
$$\bar{y}  = \hat{\theta_0} + \hat{\theta_1}\bar{x}$$

**Ordinary**:
$$\bar{y}  = \hat{\theta_0} + \sum_{i=1}^p\hat{\theta_i}\bar{x}$$

**L2 Loss**:

square of the difference, heavily penalize the outlier, would predict to the **mean** in constant model

**L1 Loss**:

take absolute value of the difference, would predict to the **median** in constant model

**Optimization**:

$$\hat{\theta} = (\mathbb{X}^T\mathbb{X})^{-1}\mathbb{X}^T\mathbb{Y}$$
$$\hat{\theta}_1 = r\frac{\sigma_y}{\sigma_x}$$
$$\hat{\theta}_0  =\bar{y} - \hat{\theta}_1\bar{x}$$

**Residual Plot**:

a good residual plot should show no pattern, otherwise some bias might present, which is not ideal.

the residuals are gauranteed to have a mean of 0 if it has an intercept term. 

## Gradient Descent

**find gradient**: 
$$ g= \frac{dL}{d\theta}\bigg|_{\theta = \theta_i}$$
**update theta**:
$$\theta_{i+1} = \theta_i - \alpha*g $$

## Bias Variance Decomposition
<center>Expected Squared Loss (Observed) = Noise + Bias^2 + Variance</center>

$$E[(y-f_\theta(x))^2] = E[(y-h_\theta(x))^2] + (h(x) - E[f_\theta(x)])^2 + E[(E[f_{\hat{\theta}}(x)] - f_{\hat{\theta}}(x))^2]$$

**increasing** model complexity will increase variance and decrease the model bias.

## Regularization

**Normal Regression**:
$$\hat{\theta} = arg \min_\theta\frac{1}{n}\sum_{i=1}^nLoss(y_i, f_\theta(x_i))$$

**Adding Regularization**:
$$\hat{\theta} = arg \min_\theta\frac{1}{n}\sum_{i=1}^nLoss(y_i, f_\theta(x_i)) + \lambda*R(\theta)$$
Larger $\lambda$ values implies more regularization weight and will lead to less complexity.

**LASSO Regularization**:
$$R(\theta) = \sum_{j=1}^d\vert\theta_j\vert$$

**Ridge Regularization**:
$$R(\theta) = \sum_{j=1}^d\theta_j^2$$

## K-Fold Validation

After the train-test split, we then separate the training set into k different sets, using the k-1 sets for training and then validate the accuracy with the set left, repeating this process for k times.

## SQL
    SELECT[columns] //must include in every query
    FROM [tables] //must include in every query
    WHERE [conditions] //filter rows
    GROUP BY [columns]
    HAVING [conditions] //filter groups
    ORDER BY [conditions]
    LIMIT [int] //show *int* rows

    Other keywords:
    LIKE [pattern] //just like regex, but simpler
    CASE [column] AS [type]
    AVG(column) // get the average of a column
    MAX(column)
    COUNT(column)

**Joins**:
* Inner joins: return columns that both tables have the record
* Left outer joins: return all columns of the left and matching columns of the right
* Right outer joins: return all columns of the right and matching columns of the left
* Outer joins: return a column if either left or right has a match
* Cross joins: return all pairs of columns

## PCA
**SVD**:
for an original matrix M with size $m \times n$, we can decompose it into:
* $\mathbb{U}$ matrix ($m \times m$): orthonormal matrix, gives the principle component by $PC = \mathbb{U} \mathbb{\Sigma}$
* $\Sigma$ matrix ($m \times n$): diagonal matrix, descending order. Note that $\sum\sigma^2 / n$ gives the variance.
* $\mathbb{V}^T$ matrix ($n \times n$): orthonormal matrix, gives the principle component by $PC = M \mathbb{V}$

**AlWAYS** center the data before doing PCA.
The principle components will tell us the direction of the maximum varaince.

## Logistic Regression
Predicts over the probability of $P(Y=1 \vert x).$

Notice that there is a linear relationship:
$$ln\bigg(\frac{P(Y=1 \vert x)}{P(Y=0 \vert x)}\bigg)$$

Solving backward to get p:
$$t =ln\frac{p}{1-p}$$
$$\implies  p = \sigma(t) = \frac{1}{1 + e^{-t}}$$
$$where\hspace{0.5cm}t = x^T\theta$$


**Cross Entropy Loss**: 
$$Loss = -y\ln\hat{y} - (1-y)\ln(1-\hat{y})$$

**Thresholding**: 
classify y to be 1 if the predicted probability is more than threshold, else classify to be 0.

**Classification Metircs**:
* True Positive: actual postive, predicted positive
* True Negative: actual negative, predicted negative
* False Positive: actual negative, predicted positive
* False Negative: actual positive, preidcted negative
  
**Classification Evaluations**:
* Accuracy: how many are actually classified correctly? $TP + TN/n$
* Precision: out of all the positives predicted, how many are acutally positive? $TP/(TP+FP)$
* Recall (True positive rate): how many actual positives are predicted to be positive? $TP/(TP+FN)$
* False Positive rate: what proportion of actual negatives are predicted to be positive? $FP/(FP+TN)$

The ROC curve plots the tradeoff between precision vs recall when we change the threshold.

## Decision Tree
**weighted entropy**:
entropy of a single node:
$$S = -\sum_{c}pc * log_2pc$$
weighted entropy is defined as entropy scaled by the fraction of sample in the node

**preventing tree growth**:
* Setting a maximum tree depth
* Setting a minimum number of samples to split a node

**Random Forests**:
Building multiple decision trees and then vote. 

## Clustering

**k-means**:
pick an arbitrary k, and then place k random points as cluster centers, then iterate for:
  1. color points according to the closest center
  2. move center to the mean of the colors
   
**Loss Function**: 
**Inertia**: the sum of the squared distance between data points and the cluster center
**Distortion**: weighted inertia based on the number of data points

**Agglomerative Clustering**: 
Every data points start to be a unique cluster, then join close data points with one another until there are only k clusters remaining.