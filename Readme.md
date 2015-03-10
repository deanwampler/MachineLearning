#Machine Learning for developers 


Most developers these days have heard of machine learning, but when trying to find an 'easy' way into this technique, most people find themselves getting scared off by the abstractness of the concept of *Machine Learning* and terms as [*regression*](http://en.wikipedia.org/wiki/Regression_analysis), [*unsupervised learning*](http://en.wikipedia.org/wiki/Unsupervised_learning), [*Probability Density Function*](http://en.wikipedia.org/wiki/Probability_density_function) and many other definitions. And then there are books such as [*An Introduction to Statistical Learning with Applications in R*](http://www-bcf.usc.edu/~gareth/ISL/) and [*Machine Learning for Hackers*](http://shop.oreilly.com/product/0636920018483.do) who use programming language [R](http://www.r-project.org) for their examples. 

However R is not really a programming language in which one writes programs for everyday use such as is done with for example Java, C#, Scala etc. This is why in this blog, Machine learning will be introduced using [Smile](https://github.com/haifengl/smile), a machine learning library that can be used both in Java and Scala. These are languages that almost every developer has worked with once during their study or career. 

The first section ['The global idea of machine learning'](#the-global-idea-of-machine-learning) contains all important concepts and notions you need to know about to get started with the practical examples that are described in the section ['Practical Examples'](#practical-examples). The section practical examples is inspired by the examples from the book [*Machine Learning for Hackers*](http://shop.oreilly.com/product/0636920018483.do). Additionally the book [Machine Learning in Action](http://www.manning.com/pharrington/) was used for validation purposes.

Note that in this blog, 'new' definitions are hyperlinked such that if you want, you **can** read more regarding that specific topic, but you are not obliged to do this in order to be able to work through the examples. 

As final note we'd like to thank the following people:

* [Haifeng Li](https://www.linkedin.com/in/haifengli) for his support and writing the awesome  and free to use library [Smile](https://github.com/haifengl/smile)
* [Erik Meijer](https://www.linkedin.com/profile/view?id=1490860) for all suggestions and supervision of the process of writing this blog
* [Richard van Heest](https://www.linkedin.com/profile/view?id=138460950) for his feedback and co-reading the blog



##The global idea of machine learning
You probably have heard about Machine learning as a concept one time or another. However, if you would have to explain what machine learning to another person, how would you do this? Think about this for a second before reading the rest of this section.

Machine learning is explained in many ways, some more accurate than others, however there is a lot of inconsistency in its definition. Where some say machine learning is generating a static model based on historical data, which then allows you to predict for future data. Others say it's a dynamic model that keeps on changing as more data is added over time.


We agree more with the dynamic definition however, but due certain limitations we explain the static model method in the examples. However we do explain how the dynamic principle would work in the subsection [dynamic machine learning](#dynamic-machine-learning).

The other subsections explain commonly used definitions and notions in the machine learning field. We advise you to read through these before starting the practical examples.


###Features
A feature (in the field of machine learning) is a property on which a [model](#model) is trained. Say for example that you classify emails as spam/ham based on the frequency of the word 'Buy' and 'Money'. Then these words are features, or part of a feature if you would combine it with more words.. If you would use machine learning to predict whether one is a friend of you, the amount of 'common' friends could be a feature. Note that in the field, sometimes features are also referred to as attributes.

###Model
When one talks about machine learning, often the term *model* is mentioned. The model is the result of any machine learning method and the algorithm used within this method. This model can be used to make predictions in [supervised](#supervised-learning), or to retrieve clusterings in [unsupervised learning](#unsupervised-learning).

###Learning methods
In the field of machine learning there are two leading ways of learning, namely [Supervised learning](http://en.wikipedia.org/wiki/Supervised_learning) and  [Unsupervised learning](http://en.wikipedia.org/wiki/Unsupervised_learning). A brief introduction is necessary when you want to use Machine learning in your applications, as picking the right machine learning approach is an important but sometimes also a little tedious process.

####Supervised Learning
The principle of supervised learning can be used to solve many problem types. In this blog however we will stick to [Classification](#classification) and [Regression](#regression) as this covers most of the problems one wants to solve in their every day application.


#####Classification
The problem of classification within the domain of Supervised learning is relatively simple. Given a set of labels, and some data that already received the correct labels, we want to be able to *predict* labels for new data that we did not label yet. However, before thinking of your data as a classification problem, you should look at what the data looks like. If there is a clear structure in the data such that you can easily draw a regression line it might be better to use a [regression](#regression) algorithm instead. Given the data does not fit to a regression line, or when performance becomes an issue, classification is a good alternative.

An example of a classification problem would be to classify emails as Ham or Spam based on their content. Given a training set in which emails are labeled Ham/Spam, a classification algorithm can be used to train a [Model](#model). This model can then be used to predict for future emails whether they are Ham or Spam. A typical example of a classification algorithm is the [K-NN algorithm](#labeling-isps-based-on-their-downupload-speed-knn-using-smile-in-scala). Another more commonly used example of a classification problem is [Classifying Email as Spam or Ham](#classifying-email-as-spam-or-ham-naive-bayes) which is also one of the examples written on this blog.

#####Regression
Regression is a lot stronger in comparison to [classification](#classification). This is because in regression you are predicting actual values, rather than labels. Let us clarify this with a short example: given a table of weights, heights, and genders, you can use [KNN](#Labeling ISPs based on their Down/Upload speed (K-NN using Smile in Scala)) to predict ones gender when given a weight and height. With this same dataset using regression, you could instead predict ones weight or height, given the gender the respective other missing parameter. 

With this extra power, comes great responsibility, thus in the working field of regression one should be very careful when generating the model. Common pitfalls are [overfitting](#overfitting), [under fitting](#under-fitting) and not taking into account how the model handles  [extrapolation](http://en.wikipedia.org/wiki/Extrapolation) and [interpolation](http://en.wikipedia.org/wiki/Interpolation).



####Unsupervised Learning
TODO: Introduction on Unsupervised learning

#####Principal Components Analysis (PCA)
Principal Components Analysis is a technique used in statistics to convert a set of correlated columns into a smaller set of uncorrelated columns, reducing the amount features of a problem.  This smaller set of columns are called the principal components. This technique is mostly used in exploratory data analysis as it reveals internal structure in the data that can not be found with eye-balling the data.

A big weakness of PCA however are outliers in the data. these heavily influence it's result, thus looking at the data on beforehand, eliminating large outliers can greatly improve its performance.


###Validation techniques
In this section we will explain some of the techniques available for model validation, and will explain some terms that are commonly used in the Machine Learning field.


#### Cross Validation
The technique of cross validation is one of the most common techniques in the field of machine learning. It's essence is to *ignore* part of your dataset while training your [model](#model), and then using the model to predict this *ignored data*. Comparing the predictions to the actual value then gives an indication of the performance of your model. 

The most important part of this cross validation is the splitting of data. You should always use the complete dataset when performing this technique. In other words you should not randomly select X datapoints for training and then randomly select X datapoints for testing, because then some datapoints can be in both sets while others might not be used at all.


#####(2 fold) Cross Validation
In 2-fold cross validation you perform a split of the data into test and training for each fold (so 2 times) and train a model using the training dataset, followed by verification with the testing set. Doing so allows you to compute the error in the predictions for the test data 2 times. These error values then should not differ significantly. If they do, either something is wrong with your data or with the features you selected for model prediction. Either way you should look into the data more and find out what is happening for your specific case, as training a model based on the data might result in a overfitted model for erroneous data.



#### Regularization
The basic idea of regularization is preventing [overfitting](#overfitting) your [model](#model) by simplifying it. Suppose your data is a polynomial function of degree 3, but your data has noise and this would cause the model to be of a higher degree. Then the model would perform poorly on new data, where as it seems to be a good model at first. Regularization hels preventing this, by simplifying the model with a certain value *lambda*. However to find the right lambda for a model is hard when you have no idea as to when the model is overfitted or not. This is why [cross validation](#cross-validation) is often used to find the best lambda fitting your model.


##### Precision
In the field of computer science we use the term precision to define the amount of items selected which are actually relevant.

This value is computed as follows:

<img src="./Images/Precision.png"/>

To give an example:

Say we have documents {aa,ab,bc,bd,ee} and we query for documents with a in the name. If our algorithm would be to return {aa,ab} the precision would be 100% intuitively. Let's verify it by filling in the formula:


<img src="./Images/PrecisionFull.png"/>

Indeed it is 100%. Next we shall show what happens if more results are returned:

<img src="./Images/PrecisionHalf.png"/>

Here the results contained the relevant results but also 2 irrelevant results. This caused the precision to decrease. However if you would calculate the [recall](#recall) for this example it would be 100%. This is how precision and recall differ from each other.

##### Recall
In the field of computer science we use the term recall to define the amount of relevant items that are retrieved for a query.

This value is computed as follows:

<img src="./Images/Recall.png"/>

To give an example:

Say we have documents {aa,ab,bc,bd,ee} and we query for documents with a in the name. If our algorithm would be to return {aa,ab} the recall would be 100% intuitively. Let's verify it by filling in the formula:


<img src="./Images/RecallFull.png"/>

Indeed it is 100%. Next we shall show what happens if not all relevant results are returned:

<img src="./Images/RecallHalf.png"/>

Here the results  only contained half of the relevant results. This caused the recall to decrease. However if you would compute the [precision](#precision) for this situation, it would result in 100% precision, because all results are relevant.

##### Prior
The prior value that belongs to a classifier given a datapoint represents the likelihood that this datapoint belongs to this classifier. 

##### Root Mean Squared Error (RMSE)
The Root Mean Squared Error (RMSE or RMSD where D is deviation) is the square root of the variance of the differences between the actual value and predicted value.
Suppose we have the following values:

| Predicted temperature | Actual temperature |  squared difference for Model | square difference for average |
| :--: | :--:| :--:| :--: | 
|10 | 12 | 4 |  7.1111 |
|20 | 17 | 9 |  5.4444 |
|15 | 15 | 0 |  0.1111 |

Then the mean of this squared difference for the model is 4.33333, and the root of this is 2.081666. So basically in average, the model predicts the values with an average error of 2.09. The lower this RMSE value is, the better the model is in its predictions. This is why in the field, when selecting features, one computes the RMSE with and without a certain feature, in order to say something about how that feature affects the performance of the model.


Additionally, because the RMSE is an absolute value, it can be normalised in order to compare models. This results in the Normalised Root Mean Square Error (NRMSE). For computing this however, you need to know the minimum and maximum value that the system can contain. Let's suppose we can have temperatures ranging from minimum of 5 to a maximum of 25 degrees, then computing the NRMSE is as follows:

<img src="./Images/Formula4.png"/>

When we fill in the actual values we get the following result:

<img src="./Images/Formula1.png"/>

Now what is this 10.45 value?  This is the error percentage the model has in average on it's datapoints.

Finally we can use RMSE to compute a value that is known in the field as **R Squared**. This value represents how good the model performs in comparison to ignoring the model and just taking the average for each value. For that you need to calculate the RMSE for the average first. This is 4.22222  (taking the mean of the values from the last column in the table), and the root is then 2.054805. The first thing you should notice is that this value is lower than that of the model. This is not a good sign, because this means the model performs **worse** than just taking the mean. However to demonstrate how to compute **R Squared** we will continue the computations.

We now have the RMSE for both the model and the mean, and then computing how well the model performs in comparison to the mean is done as follows:

<img src="./Images/Formula2.png"  />

This results in the following computation:

<img src="./Images/Formula3.png"  />

Now what does this -1.307229 represent. Basically it says that the model that predicted these values performs +/- 1.31 percent worse than returning the average each time a value is to be predicted. In other words, we could better use the average function as a predictor rather than the model in this specific case.


###Pitfalls 
This section describes some common pitfalls in applying machine learning techniques. The idea of this section is to make you aware of of these pitfalls and help you prevent actually walking into one yourself.

##### Overfitting

When fitting a function on the data, there is a possibility the data contains noise (for example by measurement errors). If you fit every point from the data exactly, you incorporate this noise into the [model](#model). This causes the model to predict really well on your test data, but relatively poor on future data.

The left image here below show how this overfitting would look like if you where to plot your data and the fitted functions, where as the right image would represent a *good fit* of the regression line through the datapoints.


<img src="./Images/OverFitting.png" width="300px" /> 
<img src="./Images/Good_Fit.png" width="300px" />

Overfitting can easily happen when applying [regression](#regression) but can just as easily be introduced in [NBayes classifications](#classifying-email-as-spam-or-ham-naive-bayes). In regression it happens with rounding, bad measurements and noisy data. In naive bayes however, it could be the features that where picked. An example for this would be classifying spam or ham while keeping all stopwords.

Overfitting can be detected by performing [validation techniques](#validation-techniques) and looking into your data's statistical features, and detecting and removing outliers.



##### Under-fitting
When you are turning your data into a model, but are leaving (a lot of) statistical data behind, this is called under-fitting. This can happen due to various reasons, such as using a wrong regression type on the data. If you have a non-linear structure in the data, and you apply linear regression, this would result in an under-fitted model. The left image here below represents a under-fitted regression line where as the right image shows a good fit regression line.

<img src="./Images/Under-fitting.png" width="300px" /> 
<img src="./Images/Good_Fit.png" width="300px" />

You can prevent under fitting by plotting the data to get insights in the underlying structure, and using [validation techniques](#validation-techniques) such as [cross validation](#2-fold-cross-validation). 

##### Curse of dimensionality
The curse of dimensionality is a collection of problems that can occur when your data size is lower than the amount of features (dimensions) you are trying to use to create your machine learning [model](#model). An example of a dimensionality curse is matrix rank deficiency. When using [Ordinary Least Squares(OLS)](http://en.wikipedia.org/wiki/Ordinary_least_squares), the underlying algorithm solves a linear system in order to build up a model. However if you have more columns than you have rows, solving this system is not possible. If this is the case, the best solution would be to get more datapoints or reduce the feature set. 

If you want to know more regarding this curse of dimensionality, [a study focussed on this issue](http://lectures.molgen.mpg.de/networkanalysis13/LDA_cancer_classif.pdf). In this study, researchers Haifeng Li, Keshu Zhang and Tao Jiang developed an algorithm that improves cancer classification with very few datapoints in comparison to [support vector machines](http://en.wikipedia.org/wiki/Support_vector_machine) and [random forests](http://en.wikipedia.org/wiki/Random_forest)


###Dynamic machine learning
In almost all literature you can find about machine learning, a static model is generated and validated, and then used for predictions / recommendations. However in practice, this alone would not make a very good machine learning application. This is why in this section we will explain how to turn a static model into a dynamic model. Since the (most optimal) implementation depends on the algorithm you are using, we will explain the concept rather than giving a practical example. Because explaining it in text only will not be very clear we first present you the whole system in a diagram which we will use to explain machine learning and how to make it dynamic.

<img src="./Images/DynamicMachineLearning.png" />

The basic idea of machine learning can be described by the following steps:

1. Gather data
2. Split the data into a testing and training set
3. Train a model (with help of a machine learning algorithm)
4. Validate the model with a validation method which takes the model and testing data
5. do predictions based on the model

In this process there are a few steps missing when it comes to actual applications in the field. These steps are in our opinion the most important steps to make a system actually learn.

The idea behind what we call dynamic machine learning is as follows: You take your predictions, combine it with user feedback and feed it back into your system to improve your dataset and model.  But as we just said we need user feedback, so how is this gained? This is actually not that hard. Let's take friend suggestions on [Facebook](https://www.facebook.com) for example. The user is presented 2 options:  'Add Friend' or 'Remove'. Based on the decision of the user, you have direct feedback regarding that prediction. 

So say you have this user feedback, then you can apply machine learning over your machine learning application to learn about the feedback that is given. This might sound a bit strange, but we will try to explain this more elaborately. However before we do this, we need to make a *disclaimer*: our description of the Facebook friend suggestion system is a 100% assumption and in no way confirmed by Facebook itself. Their systems are a secret to the outside world.

Say the system predicts based on the following features:
1. amount of common friends
2. Same hometown
3. Same age

Then you can compute a [prior](#prior) for every person on Facebook regarding the chance that he/she is a good suggestion to be your friend. Say you store the results of all these predictions for a period of time, then analysing this data on its own with machine learning allows you to improve your system. To elaborate on this, say most of our 'removed' suggestions had a high rating on feature 2, but relatively low on 1, then we can add weights to the prediction system such that feature 1 is more important than feature 2. This will then improve the recommendation system for us. 

Additionally, our dataset grows over time, so we should keep on updating our model with the new data to make the predictions more accurate. How to do this however, depends on the size and mutation rate of your data.



##Practical Examples

In this section we present you a set of machine learning algorithms in a practical setting. The idea of these examples is to get you started with machine learning algorithms without an in depth explanation of the underlying algorithms.  We focus purely on the functional aspect of there algorithms, how you can verify your implementation and warn for common pitfalls.

The following examples are available:


* [Labeling ISP's based on their down/upload speed (KNN)](#labeling-isps-based-on-their-downupload-speed-knn-using-smile-in-scala)
* [Classifying email as Spam or Ham](#classifying-email-as-spam-or-ham-naive-bayes)
* [Ranking emails based on their content (Recommendation system)](#ranking-emails-based-on-their-content-recommendation-system)
* [An attempt at rank prediction for top selling books using text regression](#an-attempt-at-rank-prediction-for-top-selling-books-using-text-regression)
* [Predicting weight based on height (Linear Regression OLS)](#predicting-weight-based-on-height-using-ordinary-least-squares)
* [Using unsupervised learning to merge features (PCA)](#using-unsupervised-learning-to-merge-features-pca)


###Labeling ISPs based on their Down/Upload speed (KNN using Smile in Scala)

The goal of this section is to use the K-NN implementation from  in Scala to classify download/upload speed pairs as [ISP](http://en.wikipedia.org/wiki/Internet_service_provider) Alpha (represented by 0) or Beta (represented by 1).  

To start with this example I assume you created a new Scala project in your favourite IDE, and downloaded and added the [Smile Machine learning](https://github.com/haifengl/smile/releases)  and its dependency [SwingX](https://java.net/downloads/swingx/releases/) to this project. As final assumption you also downloaded the [example data](https://github.com/Xyclade/MachineLearning/raw/Master/Example%20Data/KNN_Example_1.csv).

The first step is to load the CSV data file. As this is no rocket science, I provide the code for this without further explanation:

```scala

object KNNExample {
    def main(args: Array[String]): Unit = {
    val basePath = "/.../KNN_Example_1.csv"
    val testData = GetDataFromCSV(new File(basePath))    
    }
    
  def GetDataFromCSV(file: File): (Array[Array[Double]], Array[Int]) = {
    val source = scala.io.Source.fromFile(file)
    val data = source.getLines().drop(1).map(x => GetDataFromString(x)).toArray
    source.close()
    val dataPoints = data.map(x => x._1)
    val classifierArray = data.map(x => x._2)
    return (dataPoints, classifierArray)        
  }
  
  def GetDataFromString(dataString: String): (Array[Double], Int) = {

    //Split the comma separated value string into an array of strings
    val dataArray: Array[String] = dataString.split(',')

    //Extract the values from the strings
    val xCoordinate: Double = dataArray(0).toDouble
    val yCoordinate: Double = dataArray(1).toDouble
    val classifier: Int = dataArray(2).toInt

    //And return the result in a format that can later easily be used to feed to Smile
    return (Array(xCoordinate, yCoordinate), classifier)
  }
}
```

First thing you might wonder now is *why the frick is the data formatted that way*. Well, the separation between dataPoints and their label values is for easy splitting between testing and training data, and because the API expects the data this way for both executing the KNN algorithm and plotting the data. Secondly the datapoints stored as an ```Array[Array[Double]]``` is done to support datapoints in more than just 2 dimensions.

Given the data the first thing to do next is to see what the data looks like. For this Smile provides a nice Plotting library. In order to use this however, the application should be changed to a Swing application. Additionally the data should be fed to the plotting library to get a JPane with the actual plot. After changing your code it should like this:

 ```scala
 
 object KNNExample extends SimpleSwingApplication {
  def top = new MainFrame {
    title = "KNN Example"
    val basePath = "/.../KNN_Example_1.csv"

    val testData = GetDataFromCSV(new File(basePath))

    val plot = ScatterPlot.plot(testData._1, testData._2, '@', Array(Color.red, Color.blue))
    peer.setContentPane(plot)
    size = new Dimension(400, 400)

  }
  ...
  ```
 
The idea behind plotting the data is to verify whether KNN is a fitting Machine Learning algorithm for this specific set of data. In this case the data looks as follows:

<img src="./Images/KNNPlot.png" width="300px" height="300px" />

In this plot you can see  that the blue and red points seem to be mixed in the area (3 < x < 5) and (5 < y < 7.5). If the groups were not mixed at all, but were two separate clusters, a [regression](#regression) algorithm  such as in the example [Page view prediction with regression](#page-view-prediction-with-regression) could be used instead. However, since the groups are mixed the KNN algorithm is a good choice as fitting a linear decision boundary would cause a lot of false classifications in the mixed area.

Given this choice to use KNN to be a good one, lets continue with the actual Machine Learning part. For this the GUI is ditched since it does not really add any value. Recall from the section [*The global idea of Machine Learning*](#the-global-idea-of-machine-learning) that in machine learning there are 2 key parts: Prediction and Validation. First we will look at the Validation, as using a model without any validation is never a good idea. The main reason to validate the model here is to prevent [overfitting](#overfitting). However, before we even can do validation, a *correct* K should be chosen. 

The drawback is that there is no golden rule for finding the correct K. However finding a K that allows for most datapoints to be classified correctly can be done by looking at the data. Additionally The K should be picked carefully to prevent undecidability by the algorithm. Say for example ```K=2```, and the problem has 2 labels, then when a point is between both labels, which one should the algorithm pick. There is a *rule of Thumb* that K should be the square root of the number of features (on other words the number of dimensions). In our example this would be ```K=1```, but this is not really a good idea since this would lead to higher false-classifications around decision boundaries. Picking ```K=2``` would result in the error regarding our two labels, thus picking ```K=3``` seems like a good fit for now.

For this example we do [2-fold Cross Validation](http://en.wikipedia.org/wiki/Cross-validation_(statistics)). In general 2-fold Cross validation is a rather weak method of model Validation, as it splits the dataset in half and only validates twice, which still allows for overfitting, but since the dataset is only 100 points, 10-fold (which is a stronger version) does not make sense, since then there would only be 10 datapoints used for testing, which would give a skewed error rate.

```scala

  def main(args: Array[String]): Unit = {
   val basePath = "/.../KNN_Example_1.csv"
    val testData = GetDataFromCSV(new File(basePath))

    //Define the amount of rounds, in our case 2 and initialise the cross validation
    val cv = new CrossValidation(testData._2.length, validationRounds)

    val testDataWithIndices = (testData._1.zipWithIndex, testData._2.zipWithIndex)

    val trainingDPSets = cv.train
      .map(indexList => indexList
      .map(index => testDataWithIndices
      ._1.collectFirst { case (dp, `index`) => dp}.get))

    val trainingClassifierSets = cv.train
      .map(indexList => indexList
      .map(index => testDataWithIndices
      ._2.collectFirst { case (dp, `index`) => dp}.get))

    val testingDPSets = cv.test
      .map(indexList => indexList
      .map(index => testDataWithIndices
      ._1.collectFirst { case (dp, `index`) => dp}.get))

    val testingClassifierSets = cv.test
      .map(indexList => indexList
      .map(index => testDataWithIndices
      ._2.collectFirst { case (dp, `index`) => dp}.get))


    val validationRoundRecords = trainingDPSets
      .zipWithIndex.map(x => (x._1, trainingClassifierSets(x._2), testingDPSets(x._2), testingClassifierSets(x._2)))

    validationRoundRecords.foreach { record =>

      val knn = KNN.learn(record._1, record._2, 3)

      //And for each test data point make a prediction with the model
      val predictions = record._3.map(x => knn.predict(x)).zipWithIndex

      //Finally evaluate the predictions as correct or incorrect and count the amount of wrongly classified data points.
      val error = predictions.map(x => if (x._1 != record._4(x._2)) 1 else 0).sum

      println("False prediction rate: " + error / predictions.length * 100 + "%")
    }
  }
  ```
If you execute this code several times you might notice the false prediction rate to fluctuate quite a bit. This is due to the random samples taken for training and testing. When this random sample is taken a bit unfortunate, the error rate becomes much higher while when taking a good random sample, the error rate could be extremely low. 

Unfortunately I cannot provide you with a golden rule to when your model was trained with the best possible training set. One would say the model with the least error rate is always the best, but when you recall the term [overfitting](#overfitting) picking this particular model might perform really bad on future data. This is why having a large enough and representative dataset is key to a good Machine Learning application. However when aware of this issue, you could implement manners to keep updating your model based on new data and known correct classifications.


Let's suppose you implement the KNN into your application, then you should have gone through the following steps. First you took care of getting the training and testing data. Next up you generated and validated several models and picked the model which gave the best results. After these steps you can finally do predictions using your ML implementations:


```scala
 
val unknownDataPoint = Array(5.3, 4.3)
val result = knn.predict(unknownDatapoint)
if (result == 0)
{
	println("Internet Service Provider Alpha")
}
else if (result == 1)
{
	println("Internet Service Provider Beta")
}
else
{
	println("Unexpected prediction")
}

```

These predictions can then be used to present to the users of your system, for example as friend suggestion on a social networking site. The feedback the user gives on these predictions is valuable and should thus be fed into the system for updating your model.
 


###Classifying Email as Spam or Ham (Naive Bayes)
The goal of this section is to use the Naive Bayes implementation from [Smile](https://github.com/haifengl/smile) in Scala to classify emails as Spam or Ham based on their content.  

To start with this example I assume you created a new Scala project in your favourite IDE, and downloaded and added the [Smile Machine learning](https://github.com/haifengl/smile/releases) library and its dependency [SwingX](https://java.net/downloads/swingx/releases/) to this project. As final assumption you also downloaded and extracted the [example data](./Example%20Data/NaiveBayes_Example_1.zip). This example data comes from the [SpamAssasins public corpus](http://spamassasin.apache.org/publiccorpus/). 

As with every machine learning implementation, the first step is to load in the training data. However in this example we are taking it 1 step further into machine learning. In the [KNN examples](#labeling-isps-based-on-their-downupload-speed-knn-using-smile-in-scala) we had the download and upload speed as [features](#features). We did not refer to them as features, as they where the only properties available. For spam classification it is not completely trivial what to use as features. One can use the Sender, the subject, the message content, and even the time of sending as features for classifying as spam or ham.  

In this example we will use the content of the email as feature. By this we mean, we will select the features (words in this case) from the bodies of the emails in the training set. In order to be able to do this, we need to build a [Term Document Matrix (TDM)](http://en.wikipedia.org/wiki/Document-term_matrix). We could use a library for this, but in order to gain more insight, let's build it ourselves. This also gives us all freedom in changing around stuff for properly selecting features.


We will start off with writing the functions for loading the example data. This will be done with a ```getMessage``` method which gets a filtered body from an email given a filename as parameter.

```scala

def getMessage(file : File)  : String  =
  {
    //Note that the encoding of the example files is latin1, thus this should be passed to the from file method.
    val source = scala.io.Source.fromFile(file)("latin1")
    val lines = source.getLines mkString "\n"
    source.close()
    //Find the first line break in the email, as this indicates the message body
    val firstLineBreak = lines.indexOf("\n\n")
    //Return the message body filtered by only text from a-z and to lower case 
    return lines.substring(firstLineBreak).replace("\n"," ").replaceAll("[^a-zA-Z ]","").toLowerCase()
  }
```

Now we need a method that gets all the filenames for the emails, from the example data folder structure that we provided you with.

```scala
  
  def getFilesFromDir(path: String):List[File] = {
    val d = new File(path)
    if (d.exists && d.isDirectory) {
      //Remove the mac os basic storage file, and alternatively for unix systems "cmds"
      d.listFiles.filter(  x => x.isFile &&  !x.toString.contains(".DS_Store") && ! x.toString.contains("cmds")).toList
    } 
    else {
      List[File]()
    }
  }
```

And finally lets define a set of paths that make it easier to load the different datasets from the example data. Together with this we also directly define a sample size of 500, as this is the complete amount of training emails are available for the spam set. We take the same amount of ham emails as the training set should be balanced for these two classification groups.

```scala
  
  def main(args: Array[String]): Unit = {
    val basePath = "/Users/../Downloads/data"
    val spamPath = basePath + "/spam"
    val spam2Path = basePath + "/spam_2"
    val easyHamPath = basePath + "/easy_ham"
    val easyHam2Path = basePath + "/easy_ham_2"
    val hardHamPath = basePath + "/hard_ham"
    val hardHam2Path = basePath + "/hard_ham_2"

  	val amountOfSamplesPerSet = 500
    val amountOfFeaturesToTake = 100   
    //First get a subset of the filenames for the spam sample set (500 is the complete set in this case)
    val listOfSpamFiles =   getFilesFromDir(spamPath).take(amountOfSamplesPerSet)
    //Then get the messages that are contained in these files
  	val spamMails = listOfSpamFiles.map(x => (x, getMessage(x)))
    
     //Get a subset of the filenames from the ham sampleset (note that in this case it is not necessary to randomly sample as the emails are already randomly ordered)
  	val listOfHamFiles =   getFilesFromDir(easyHamPath).take(amountOfSamplesPerSet)
  	//Get the messages that are contained in the ham files
  	val hamMails  = listOfHamFiles.map{x => (x,getMessage(x)) }
  }

  
```


Now that we have the training data for both the ham and the spam email, we can start building 2 [TDM's](http://en.wikipedia.org/wiki/Document-term_matrix). But before we show you the code for this, lets first explain shortly why we actually need this. The TDM will contain **ALL** words which are contained in the bodies of the training set, including frequency rate. However, since frequency might not be the best measure (as 1 email which contains 1.000.000 times the word 'pie' would mess up the complete table) we will also compute the **occurrence rate**. By this we mean, the amount of documents that contain that specific term. So lets start off with generating the two TDM's.


```scala

 val spamTDM  = spamMails
      .flatMap(email => email
        ._2.split(" ")
          .filter(word => word.nonEmpty)
            .map(word => (email._1.getName,word)))
              .groupBy(x => x._2)
                .map(x => (x._1, x._2.groupBy(x => x._1)))
                  .map(x => (x._1, x._2.map( y => (y._1, y._2.length)))).toList

    //Sort the words by occurrence rate  descending (amount of times the word occurs among all documents)
  val sortedSpamTDM = spamTDM.sortBy(x =>  - (x._2.size.toDouble / spamMails.length))
  val hamTDM  = hamMails
      .flatMap(email => email
      ._2.split(" ")
      .filter(word => word.nonEmpty)
      .map(word => (email._1.getName,word)))
      .groupBy(x => x._2)
      .map(x => (x._1, x._2.groupBy(x => x._1)))
      .map(x => (x._1, x._2.map( y => (y._1, y._2.length)))).toList

    //Sort the words by occurrence rate  descending (amount of times the word occurs among all documents)
    val sortedHamTDM = hamTDM.sortBy(x =>  - (x._2.size.toDouble / spamMails.length))

```

Given the tables, lets take a look at the top 50 words for each table. Note that the red words are from the spam table and the green words are from the ham table. Additionally, the size of the words represents the occurrence rate. Thus the larger the word, the more documents contained that word at least once.

<img src="./Images/Ham_Stopwords.png" width="400px" height="200px" />
<img src="./Images/Spam_Stopwords.png" width="400px" height="200px" />

As you can see, mostly stop words come forward. These stopwords are noise, which we should not use in our feature selection, this we should remove these from the tables before selecting the features. We've included a list of stopwords in the example dataset. Lets first define the code to get these stopwords.
```scala
  def getStopWords() : List[String] =
  {
    val source = scala.io.Source.fromFile(new File("/Users/.../.../Example Data/stopwords.txt"))("latin1")
    val lines = source.mkString.split("\n")
    source.close()
    return  lines.toList
  }
 
```

Now we can expand the TDM generation code with removing the stopwords from the intermediate results:

```scala

val stopWords = getStopWords

val spamTDM  = spamMails
      .flatMap(email => email
        ._2.split(" ")
          .filter(word => word.nonEmpty && !stopWords.contains(word))
            .map(word => (email._1.getName,word)))
              .groupBy(x => x._2)
                .map(x => (x._1, x._2.groupBy(x => x._1)))
                  .map(x => (x._1, x._2.map( y => (y._1, y._2.length)))).toList


 val hamTDM  = hamMails
      .flatMap(email => email
      ._2.split(" ")
      .filter(word => word.nonEmpty && !stopWords.contains(word))
      .map(word => (email._1.getName,word)))
      .groupBy(x => x._2)
      .map(x => (x._1, x._2.groupBy(x => x._1)))
      .map(x => (x._1, x._2.map( y => (y._1, y._2.length)))).toList

```

If we once again look at the top 50 words for spam and ham, we see that most of the stopwords are gone. We could fine-tune more, but for now lets go with this.

<img src="./Images/Ham_No_Stopwords.png" width="400px" height="200px" />
<img src="./Images/Spam_No_Stopwords.png" width="400px" height="200px" />

With this insight in what 'spammy' words and what typical 'ham-words' are, we can decide on building a feature-set which we can then use in the Naive Bayes algorithm for creating the classifier. Note: it is always better to include **more** features, however performance might become an issue when having all words as features. This is why in the field, developers tend to drop features that do not have a significant impact, purely for performance reasons. Alternatively machine learning is done running complete [Hadoop](http://hadoop.apache.org/) clusters, but explaining this would be outside the scope of this blog.

For now we will select the top 100 spammy words based on occurrence(thus not frequency) and do the same for ham words and combine this into 1 set of words which we can feed into the bayes algorithm. Finally we also convert the training data to fit the input of the Bayes algorithm. Note that the final feature set thus is 200 - (#intersecting words *2). Feel free to experiment with higher and lower feature counts.


```scala

//Add the code for getting the TDM data and combining it into a feature bag.
val hamFeatures = hamTDM.records.take(amountOfFeaturesToTake).map(x => x.term)
val spamFeatures = spamTDM.records.take(amountOfFeaturesToTake).map(x => x.term)

//Now we have a set of ham and spam features, we group them and then remove the intersecting features, as these are noise.
var data = (hamFeatures ++ spamFeatures).toSet
hamFeatures.intersect(spamFeatures).foreach(x => data = (data - x))

//Initialise a bag of words that takes the top x features from both spam and ham and combines them
var bag = new Bag[String] (data.toArray)
//Initialise the classifier array with first a set of 0(spam) and then a set of 1(ham) values that represent the emails
var classifiers =  Array.fill[Int](amountOfSamplesPerSet)(0) ++  Array.fill[Int](amountOfSamplesPerSet)(1)

//Get the trainingData in the right format for the spam mails
var spamData = spamMails.map(x => bag.feature(x._2.split(" "))).toArray

//Get the trainingData in the right format for the ham mails
var hamData = hamMails.map(x => bag.feature(x._2.split(" "))).toArray

//Combine the training data from both categories
var trainingData = spamData ++ hamData
```

Given this feature bag, and a set of training data, we can start training the algorithm. For this we can chose a few different models: 'general', 'multinomial' and Bernoulli. In this example we focus on the multinomial but feel free to try out the other model types as well.


```scala
//Create the bayes model as a multinomial with 2 classification groups and the amount of features passed in the constructor.
  var bayes = new NaiveBayes(NaiveBayes.Model.MULTINOMIAL, 2, data.size)
  //Now train the bayes instance with the training data, which is represented in a specific format due to the bag.feature method, and the known classifiers.

  bayes.learn(trainingData, classifiers)
```

Now that we have the trained model, we can once again do some validation. However, in the example data we already made a separation between easy and hard ham, and spam, thus we will not apply the cross validation, but rather validate the model these test sets. We will start with validation of spam classification. For this we use the 1397 spam emails from the spam2 folder.

```scala
val listOfSpam2Files =   getFilesFromDir(spam2Path)
val spam2Mails = listOfSpam2Files.map{x => (x,getMessage(x)) }
val spam2FeatureVectors = spam2Mails.map(x => bag.feature(x._2.split(" ")))
val spam2ClassificationResults = spam2FeatureVectors.map(x => bayes.predict(x))

//Correct classifications are those who resulted in a spam classification (0)
val correctClassifications = spam2ClassificationResults.count( x=> x == 0)
println(correctClassifications + " of " + listOfSpam2Files.length + "were correctly classified")
println(((correctClassifications.toDouble /  listOfSpam2Files.length) * 100)  + "% was correctly classified")

//In case the algorithm could not decide which category the email belongs to, it gives a -1 (unknown) rather than a 0 (spam) or 1 (ham)
val unknownClassifications = spam2ClassificationResults.count( x=> x == -1)
println(unknownClassifications + " of " + listOfSpam2Files.length + "were unknowingly classified")
println(((unknownClassifications.toDouble /  listOfSpam2Files.length) * 100)  + "% was unknowingly classified")

```

If we run this code several times with different feature amounts we get the following results:


| amountOfFeaturesToTake	| Spam (Correct)| Unknown| Ham | 
| ---------------------		|:-------------	| :----- |:----|
| 50      					| 1281 (91.70%)	| 16 (1.15%)	| 100 (7.15%) |
| 100     					| 1189 (85.11%)	| 18 (1.29%)	| 190 (13.6%)|
| 200     					| 1197 (85.68%)	| 16 (1.15%)	| 184 (13.17%)|
| 400     					| 1219 (87.26%)	| 13 (0.93%)	| 165 (11.81%)|

Interestingly enough, the algorithm works best with only 50 features. However, if you recall that there were still *stop words* in the top 50 classification terms which could explain this result.  If you look at how the values change as the amount of features increase (starting at 100), you can see that with more features, the overall result increases. Note that there are a group of unknown emails. For these emails the [prior](#prior) was equal for both classes. Note that this also is the case if there are no feature words for ham nor spam in the email, because then the algorithm would classify it 50% ham 50% spam.


If we change the path to ```easyHam2Path``` and rerun the code for we get the following results:

| amountOfFeaturesToTake	| Spam | Unknown| Ham  (Correct) | 
| ---------------------		|:-------------	| :----- |:----|
| 50      					| 120 (8.57%)	| 28 ( 2.0%)	| 1252 (89.43%)	|
| 100   					| 44 (3.14%)	| 11 (0.79%)	| 1345 (96.07%)	|
| 200 						| 36 (2.57%)	| 7 (0.5%)	| 1357 (96.93%)	|
| 400     					| 24 (1.71%)	| 7 (0.5%)	| 1369 (97.79%) |

Here we see that indeed, when you use only 50 features, the amount of ham that gets classified correctly is significantly lower in comparison to the correct classifications when using 100 features.

We could work through the hard ham, but since the building bricks are already here, we leave this to the reader. 

###Ranking emails based on their content (Recommendation system)
This example will be completely about building your own recommendation system. We will use a subset of the email data which we used in the example [Classifying Email as Spam or Ham](#classifying-email-as-spam-or-ham-naive-bayes). This subset can be downloaded [here](https://github.com/Xyclade/MachineLearning/raw/Master/Example%20Data/Recommendation_Example_1.zip). Note that this is a set of received emails, thus we lack 1 half of the data, namely the outgoing emails of this mailbox. However even without this information we can do some pretty neat ranking as we will see later on.

For this example, as usual we expect you to have created a new Scala project in your favourite IDE, and downloaded and added the [Smile Machine learning](https://github.com/haifengl/smile/releases) library and its dependency [SwingX](https://java.net/downloads/swingx/releases/) to this project.

The first thing to do is to determine what features we will base our ranking on. When building your own recommendation system this is one of the hardest parts. Coming up with good features is not trivial, and when you finally selected features the data might not be directly usable for these features. 

This is why we will go into detail a bit more on how we select the features and prepare the data. However before we can get to this step, its time to extract as much data as we can from our email set. Since the data is a bit tedious in it's format we provide the code to do this. The inline comments explain why things are done the way they are. Note that the application is a swing application with a GUI from the start. We do this because we will need to plot data in order to gain insight later on. Also note that we directly made a split in testing and training data such that we can later on test our model.


```scala

object RecommendationSystem extends SimpleSwingApplication {

  def top = new MainFrame {
    title = "Recommendation System Example"

    val basePath = "/Users/../data"
    val easyHamPath = basePath + "/easy_ham"

    val mails = getFilesFromDir(easyHamPath).map(x => getFullEmail(x))
    val timeSortedMails = mails
      .map(x => (getDateFromEmail(x), getSenderFromEmail(x), getSubjectFromEmail(x), getMessageBodyFromEmail(x)))
      .sortBy(x => x._1)

    val (trainingData, testingData) = timeSortedMails
      .splitAt(timeSortedMails.length / 2)
      
      }

  def getFilesFromDir(path: String): List[File] = {
    val d = new File(path)
    if (d.exists && d.isDirectory) {
      //Remove the mac os basic storage file, and alternatively for unix systems "cmds"
      d.listFiles.filter(x => x.isFile && !x.toString.contains(".DS_Store") && !x.toString.contains("cmds")).toList
    } else {
      List[File]()
    }
  }


  def getFullEmail(file: File): String = {
    //Note that the encoding of the example files is latin1, thus this should be passed to the from file method.
    val source = scala.io.Source.fromFile(file)("latin1")
    val fullEmail = source.getLines mkString "\n"
    source.close()

    fullEmail
  }


  def getSubjectFromEmail(email: String): String = {

    //Find the index of the end of the subject line
    val subjectIndex = email.indexOf("Subject:")
    val endOfSubjectIndex = email.substring(subjectIndex).indexOf('\n') + subjectIndex

    //Extract the subject: start of subject + 7 (length of Subject:) until the end of the line.
    val subject = email
    .substring(subjectIndex + 8, endOfSubjectIndex)
    .trim
    .toLowerCase

    //Additionally, we check whether the email was a response and remove the 're: ' tag, to make grouping on topic easier:
    subject.replace("re: ", "")
  }

  def getMessageBodyFromEmail(email: String): String = {

    val firstLineBreak = email.indexOf("\n\n")
    //Return the message body filtered by only text from a-z and to lower case
    email.substring(firstLineBreak)
    .replace("\n", " ")
    .replaceAll("[^a-zA-Z ]", "")
    .toLowerCase
  }


  def getSenderFromEmail(email: String): String = {
    //Find the index of the From: line
    val fromLineIndex = email.indexOf("From:")
    val endOfLine = email.substring(fromLineIndex).indexOf('\n') + fromLineIndex

    //Search for the <> tags in this line, as if they are there, the email address is contained inside these tags
    val mailAddressStartIndex = email
    .substring(fromLineIndex, endOfLine)
    .indexOf('<') + fromLineIndex + 1
    val mailAddressEndIndex = email
    .substring(fromLineIndex, endOfLine)
    .indexOf('>') + fromLineIndex

    if (mailAddressStartIndex > mailAddressEndIndex) {

      //The email address was not embedded in <> tags, extract the substring without extra spacing and to lower case
      var emailString = email
      .substring(fromLineIndex + 5, endOfLine)
      .trim
      .toLowerCase

      //Remove a possible name embedded in () at the end of the line, for example in test@test.com (tester) the name would be removed here
      val additionalNameStartIndex = emailString.indexOf('(')
      if (additionalNameStartIndex == -1) {
        emailString
        .toLowerCase
      }
      else {
        emailString
        .substring(0, additionalNameStartIndex)
        .trim
        .toLowerCase
      }
    }
    else {
      //Extract the email address from the tags. If these <> tags are there, there is no () with a name in the From: string in our data
      email
      .substring(mailAddressStartIndex, mailAddressEndIndex)
      .trim
      .toLowerCase
    }
  }

  def getDateFromEmail(email: String): Date = {
    //Find the index of the Date: line in the complete email
    val dateLineIndex = email.indexOf("Date:")
    val endOfDateLine = email.substring(dateLineIndex).indexOf('\n') + dateLineIndex

    //All possible date patterns in the emails.
    val datePatterns = Array("EEE MMM dd HH:mm:ss yyyy", "EEE, dd MMM yyyy HH:mm", "dd MMM yyyy HH:mm:ss", "EEE MMM dd yyyy HH:mm")

    datePatterns.foreach { x =>
      //Try to directly return a date from the formatting.when it fails on a pattern it continues with the next one until one works
      Try(return new SimpleDateFormat(x).parse(email.substring(dateLineIndex + 5, endOfDateLine).trim.substring(0, x.length)))
    }
    //Finally, if all failed return null (this will not happen with our example data but without this return the code will not compile)
    null
  }
}

```

This pre-processing of the data is very common and can be a real pain when your data is not standardised such as with the dates and senders of these emails. However given this chunk of code we now have available the following properties of our example data:(full email, receiving date, sender, subject, body). This allows us to continue working on the actual features to use in the recommendation system. 

The first recommendation feature we will make is based on the sender of the email. Those whom you receive more emails from should be ranked higher than ones you get less email from. This is a strong assumption, but instinctively you will probably agree, given the fact that spam is left out. Let's look at the distribution of senders over the complete email set.


```scala

//Add to the top body:



//First we group the emails by Sender, then we extract only the sender address and amount of emails, and finally we sort them on amounts ascending
val mailsGroupedBySender = trainingData
.groupBy(x => x._3)
.map(x => (x._1, x._2.length))
.toArray
.sortBy(x => x._2)

//In order to plot the data we split the values from the addresses as this is how the plotting library accepts the data.
val senderDescriptions = mailsGroupedBySender.map(x => x._1)
val senderValues = mailsGroupedBySender.map(x => x._2.toDouble)

val barPlot = BarPlot.plot("", senderValues, senderDescriptions)
   
//Rotate the email addresses by -80 degrees such that we can read them
barPlot.getAxis(0).setRotation(-1.3962634)
barPlot.setAxisLabel(0, "")
barPlot.setAxisLabel(1, "Amount of emails received ")
peer.setContentPane(barPlot)

bounds = new Rectangle(800, 600)

```

<img src="./Images/Mail_per_Sender_Distribution.png" width="400px" /> 

Here you can see that the most frequent sender sent 45 emails, followed by 37 emails and then it goes down rapidly. Due to these huge outliers, directly using this data would result in the highest 1 or 2 senders to be rated as very important where as the rest would not be considered in the recommendation system. In order to prevent this behaviour we will re-scale the data  by taking ```log1p```. The addition of 1 is to prevent trouble when taking the log value for senders who sent only 1 email. After taking the log the data looks like this.

```scala

//Code changes:
val mailsGroupedBySender = trainingData
.groupBy(x => x._3)
.map(x => (x._1, Math.log1p(x._2.length)))
.toArray
.sortBy(x => x._2)

barPlot.setAxisLabel(1, "Amount of emails received on log Scale ")

```

<img src="./Images/Mail_per_Sender_log_Distribution.png" width="400px" />

Effectively the data is still the same, however it is represented on a different scale.
Notice here that the numeric values now range between 0.69 and 3.83. This range is much smaller, causing the outliers to not skew away the rest of the data. This data manipulation trick is very common in the field of machine learning. Finding the right scale requires some insight. This is why using the plotting library of [Smile](https://github.com/haifengl/smile) to make several plots on different scales can help a lot when performing this rescaling.

The next feature we will work on is the frequency and timeframe in which subjects occur. If a subject occurs more it is likely to be of higher importance. Additionally we take into account the timespan of the thread. So the frequency of a subject will be normalised with the timespan of the emails of this subject. This makes highly active email threads come up on top. Again this is an assumption we make on which emails should be ranked higher.
 
Let's have a look at the subjects and their occurrence counts:

```scala
//Add to 'def top'  
val mailsGroupedByThread = trainingData
	.groupBy(x => x._4)

//Create a list of tuples with (subject, list of emails)
val threadBarPlotData = mailsGroupedByThread
	.map(x => (x._1, x._2.length))
	.toArray
	.sortBy(x => x._2)
	
val threadDescriptions = threadBarPlotData
	.map(x => x._1)
val threadValues = threadBarPlotData
	.map(x => x._2.toDouble)

//Code changes in 'def top'
val barPlot = BarPlot.plot("Amount of emails per subject", threadValues, threadDescriptions)
barPlot.setAxisLabel(1, "Amount of emails per subject")

```

<img src="./Images/Mail_per_Subject_Distribution.png" width="400px" />

We see a similar distribution as with the senders, so let's apply the `log1p` once more.

```scala

//Code change:
val threadBarPlotData = mailsGroupedByThread
	.map(x => (x._1, Math.log1p(x._2.length)))
	.toArray
	.sortBy(x => x._2)
```
<img src="./Images/Mail_per_Subject_log_Distribution.png" width="400px" />

Here the value's now range between 0.69 and 3.41, which is a lot better than a range of 1 to 29 for the recommendation system. However we did not incorporate the time frame yet, thus we go back to the normal frequency and apply the transformation later on. To be able to do this, we first need to get the time between the first and last thread:

```scala

//Create a list of tuples with (subject, list of emails, time difference between first and last email)
    val mailGroupsWithMinMaxDates = mailsGroupedByThread
    .map(x => (x._1, x._2, (x._2
    							.maxBy(x => x._2)
    							._1.getTime - x._2    												.minBy(x => x._2)    												._1.getTime
    						) / 1000))

 //turn into a list of tuples with (topic, list of emails, time difference, and weight) filtered that only threads occur
    val threadGroupsWithWeights = mailGroupsWithMinMaxDates
    .filter(x => x._3 != 0)
    .map(x => (x._1, x._2, x._3, 10 + Math.log10(x._2.length.toDouble / x._3)))


```

Note how we determine the difference between the min and the max, and divide it by 1000. This is to scale the time value from MS to seconds. Additionally we compute the weights by taking the frequency of a subject  and dividing it by the time difference. Since this value is very small, we want to rescale it up a little, which is done by taking the 10log. This however causes our values to become negative, which is why we add a basic value of 10 to make every value positive. The end result of this weighting is as follows:

<img src="./Images/Weighted_Subject_Distribution.png" width="400px" />

We see our values ranging roughly between (4.4 and 8.6) which shows that outliers do not largely influence the feature anymore. Additionally we will look at the top 10 vs bottom 10 weights to get some more insight in what happened.

**Top 10 weights:**

| Subject 	| Frequency	| Time frame (seconds) 	| Weight |
| :-- 		| :-- 		| :-- 			| :-- | 
|[ilug] what howtos for soho system | 2 | 60  | 8.522878745280337 |
|[zzzzteana] the new steve earle  | 2  | 120 | 8.221848749616356 | 
| [ilug] looking for a file / directory in zip file | 2 | 240 | 7.920818753952375 |
| ouch... [bebergflame]| 2 |300  | 7.823908740944319|
| [ilug] serial number in hosts file | 2 | 420 |  7.6777807052660805 |
| [ilug] email list management howto | 3 | 720 |  7.619788758288394 | 
| should mplayer be build with win32 codecs?  | 2 | 660 |  7.481486060122112 |
| [spambayes] all cap or cap word subjects  | 2 | 670 |  7.474955192963154 |
| [zzzzteana] save the planet, kill the people  | 3 | 1020 |  7.468521082957745 |
| [ilug] got me a crappy laptop  | 2 | 720 | 7.443697499232712 |


**Bottom 10 weights:**

| Subject 	| Frequency	| Time frame (seconds) 	| Weight |
| :-- 		| :-- 		| :-- 			| :-- | 
|secure sofware key | 14  | 1822200 | 4.885531993376329 | 
|[satalk] help with postfix + spamassassin | 2  | 264480 | 4.878637159436609 | 
|<nettime> the war prayer | 2  | 301800 | 4.82131076022441 | 
|gecko adhesion finally sussed. | 5  | 767287 | 4.814012164243057 | 
|the mime information you requested (last changed 3154 feb 14) | 3  | 504420 | 4.774328956918856 | 
|use of base image / delta image for automated recovery from | 5  | 1405800 | 4.551046465355412 | 
|sprint delivers the next big thing?? | 5  | 1415280 | 4.548127634846606 | 
|[razor-users] collision of hashes? | 4  | 1230420 | 4.512006609524829 | 
|[ilug] modem problems | 2  | 709500 | 4.450077595870488 | 
|tech's major decline | 2  | 747660 | 4.427325849260936 | 


As you can see the highest weights are given to emails which almost instantly got a follow up email response, where as the lowest weights are given to emails with very long timeframes. This allows for emails with a very low frequency to still be rated as very important based on the timeframe in which they were sent. With this we have 2 features: the amount of emails from a sender ```mailsGroupedBySender```, and the weight of emails that belong to an existing thread ```threadGroupsWithWeights```.


Let's continue with the next feature, as we want to base our ranking on as much features as possible. This next feature will be based on the weight ranking that we just computed. The idea is that new emails with different subjects will arrive. However, chances are that they contain keywords that are similar to earlier received important subjects. This will allow us to rank emails as important before a thread (multiple messages with the same subject) was started. For that we specify the weight of the keywords to be the weight of the subject in which the term occurred. If this term occurred in multiple threads, we take the highest weight as the leading one.

There is one issue with this feature, which are stopwords. Luckily we have a stopwords file that allows us to remove (most) english stopwords for now. However, when designing your own system you should take into account that multiple languages can occur, thus you should remove stopwords for all languages that can occur in the system. The code for this feature is as follows:


```scala

  def getStopWords: List[String] = {
    val source = scala.io.Source.fromFile(new File("/Users/../stopwords.txt"))("latin1")
    val lines = source.mkString.split("\n")
    source.close()
    lines.toList
   }

//Add to top:
val stopWords = getStopWords

val threadTermWeights =  threadGroupsWithWeights
	.toArray
	.sortBy(x => x._4)
	.flatMap(x => x._1
			.replaceAll("[^a-zA-Z ]", "")
			.toLowerCase.split(" ")
			.filter(_.nonEmpty)
			.map(y => (y,x._4)))
			
val filteredThreadTermWeights = threadTermWeights
	.groupBy(x => x._1)
	.map(x => (x._1, x._2.maxBy(y => y._2)._2))
	.toArray.sortBy(x => x._1)
	.filter(x => !stopWords.contains(x._1))

```
Given this code we now have a list ```filteredThreadTermWeights``` of terms including weights that are based on existing threads. These weights can be used to compute the weight of the subject of a new email even if the email is not a response to an existing thread.

As fourth feature we want to incorporate weighting based on the terms that are occurring with a high frequency in all the emails. For this we build up a [TDM](http://en.wikipedia.org/wiki/Document-term_matrix), but this time the TDM is a bit different as in the former examples, as we only log the frequency of the terms in all documents. Furthermore we take the log10 of the occurrence counts. This allows us to scale down the term frequencies such that the results do not get affected by possible outlier values.

```scala

val tdm = trainingData
      .flatMap(x => x._4.split(" "))
      .filter(x => x.nonEmpty && !stopWords.contains(x))
      .groupBy(x => x)
      .map(x => (x._1, Math.log10(x._2.length + 1)))
      .filter(x => x._2 != 0)

```

This ```tdm``` list  allows us to compute an importance weight for the email body of new emails based on historical data 

With these preparations for our 4 features, we can make our actual ranking calculation for the training data as follows:


```scala
val trainingRanks = trainingData.map(mail => {
      //mail contains (full content, date, sender, subject, body)

      //Determine the weight of the sender
      val senderWeight = mailsGroupedBySender
        .collectFirst { case (mail._2, x) => x}
        .getOrElse(1.0)

      //Determine the weight of the subject
      val termsInSubject = mail._3
        .replaceAll("[^a-zA-Z ]", "")
        .toLowerCase.split(" ")
        .filter(x => x.nonEmpty && !stopWords.contains(x))

      val termWeight = if (termsInSubject.size > 0) termsInSubject
        .map(x => {
        tdm.collectFirst { case (y, z) if y == x => z + 1}
          .getOrElse(1.0)
      })
        .sum / termsInSubject.length
      else 1.0

      //Determine if the email is from a thread, and if it is the weight from this thread:
      val threadGroupWeight: Double = threadGroupsWithWeights
        .collectFirst { case (mail._3, _, _, weight) => weight}
        .getOrElse(1.0)

      //Determine the commonly used terms in the email and the weight belonging to it:
      val termsInMailBody = mail._4
        .replaceAll("[^a-zA-Z ]", "")
        .toLowerCase.split(" ")
        .filter(x => x.nonEmpty && !stopWords.contains(x))

      val commonTermsWeight = if (termsInMailBody.size > 0) termsInMailBody
        .map(x => {
        tdm.collectFirst { case (y, z) if y == x => z + 1}
          .getOrElse(1.0)
      })
        .sum / termsInMailBody.length
      else 1.0

      val rank = termWeight * threadGroupWeight * commonTermsWeight * senderWeight

      (mail, rank)
    })
    
    val median = sortedTrainingRanks(sortedTrainingRanks.length / 2)._2
    val mean = sortedTrainingRanks.map(x => x._2).sum / sortedTrainingRanks.length
```

This gives us a list ```trainingRanks``` which contains all training emails and their respective ranks.  Now how is this useful, since we want to rank new emails? The idea behind this is that sorting emails purely based on rank would not be really useful, as sorting on date is a way more intuitive way for sorting email. This is why we suggest placing a flag 'important' as a more intuitive way to represent important emails. In order to compute when this flag should be set you also need the ranks from the training set, and define a decision boundary.. 

For this decision boundary we choose the 'mean' of the sorted list. This might not work out well in practice, but since we cannot evaluate the actual importance of the emails as we are not the actual recipient, this is the best we can do. Usually the average might result in 50% ending up as important, but due to the structure of our data, this is not the case in this example.

To see how much of the testing emails are ranked priority, we add the following code:

```scala
  val testingRanks = testingData.map(mail => {
      //mail contains (full content, date, sender, subject, body)

      //Determine the weight of the sender
      val senderWeight = mailsGroupedBySender
        .collectFirst { case (mail._2, x) => x}
        .getOrElse(1.0)

      //Determine the weight of the subject
      val termsInSubject = mail._3
        .replaceAll("[^a-zA-Z ]", "")
        .toLowerCase.split(" ")
        .filter(x => x.nonEmpty && !stopWords.contains(x))

      val termWeight = if (termsInSubject.size > 0) termsInSubject
        .map(x => {
        tdm.collectFirst { case (y, z) if y == x => z + 1}
          .getOrElse(1.0)
      })
        .sum / termsInSubject.length
      else 1.0

      //Determine if the email is from a thread, and if it is the weight from this thread:
      val threadGroupWeight: Double = threadGroupsWithWeights
        .collectFirst { case (mail._3, _, _, weight) => weight}
        .getOrElse(1.0)

      //Determine the commonly used terms in the email and the weight belonging to it:
      val termsInMailBody = mail._4
        .replaceAll("[^a-zA-Z ]", "")
        .toLowerCase.split(" ")
        .filter(x => x.nonEmpty && !stopWords.contains(x))

      val commonTermsWeight = if (termsInMailBody.size > 0) termsInMailBody
        .map(x => {
        tdm.collectFirst { case (y, z) if y == x => z + 1}
          .getOrElse(1.0)
      })
        .sum / termsInMailBody.length
      else 1.0

      val rank = termWeight * threadGroupWeight * commonTermsWeight * senderWeight

      (mail, rank)
    })

    val priorityEmails = testingRanks.filter(x => x._2 >= mean)

    println(priorityEmails.length + " ranked as priority")

```

After actually running this test code, you will see that the amount of emails ranked as priority from the test set is actually  204. This is 16.32% of the test email set. To gain a bit more insight we will show the top 10 for the priority labeled emails.

| Date | Sender  | Subject  | Rank |
| :-- | :-- | :--  | :-- | 
| Wed Sep 25 12:57:00 CEST 2002 | whit@transpect.com | [razor-users] "no razor servers available at this time" | 9.142285151868377 |
| Tue Oct 01 06:18:00 CEST 2002 | angles@aminvestments.com | use new apt to do null to rh8 upgrade? | 9.03879575228188 |
| Thu Oct 03 08:02:00 CEST 2002 | rssfeeds@spamassassin.taint.org | perl.  it's just a language. | 8.859240257643194 |
| Mon Sep 30 11:11:00 CEST 2002 | axel.thimm@physik.fu-berlin.de | all set for red hat linux 8.0 | 8.844148047621394 |
| Thu Sep 26 02:00:00 CEST 2002 | pudge@perl.org | [use perl] headlines for 2002-09-26 | 8.765830978559473 |
| Thu Oct 03 12:33:00 CEST 2002 | whit@transpect.com | [razor-users] "no razor servers available at this time" | 8.739690791945359 |
| Mon Oct 07 15:22:00 CEST 2002 | blue@rocinante.com | [razor-users] razor2 error: can't find "new" | 8.734496269696358 |
| Tue Oct 01 02:00:00 CEST 2002 | pudge@perl.org | [use perl] headlines for 2002-10-01 | 8.697201448679307 |
| Tue Oct 01 02:00:00 CEST 2002 | pudge@perl.org | [use perl] stories for 2002-10-01 | 8.596675141637018 |
| Mon Sep 23 10:32:49 CEST 2002 | peter.peltonen@iki.fi | new testing packages | 8.596151383796718 |

In this top 10, based on the subjects we can say that these emails are important, however this is no formal proof of the actual importance of these emails. Validating a ranker like this is rather hard when you have no ground truth. One of the most common ways of validating and improving it is by actually presenting it to the user and letting him/her mark correct mistakes. These corrections can then be used to improve the system.

This concludes the recommendation system example.



###Predicting weight based on height (using Ordinary Least Squares)
In this section we will introduce the [Ordinary Least Squares](http://en.wikipedia.org/wiki/Ordinary_least_squares) technique which is a form of linear regression. As this technique is quite powerful, it is important to have read [regression](#regression) and the common pitfalls before starting with this example. We will cover some of these issues in this section, while others are shown in the sections [under-fitting](#under-fitting) and [overfitting](#overfitting)


As always, the first thing to do is to import a dataset. For this we provide you with the following [csv file](./Example%20Data/OLS_Regression_Example_3.csv) and code for reading this file:


```scala

  def GetDataFromCSV(file: File): (Array[Array[Double]], Array[Double]) = {
    val source = scala.io.Source.fromFile(file)
    val data = source.getLines().drop(1).map(x => GetDataFromString(x)).toArray
    source.close()
    var inputData = data.map(x => x._1)
    var resultData = data.map(x => x._2)

    return (inputData,resultData)
  }

  def GetDataFromString(dataString: String): (Array[Double], Double) = {

    //Split the comma separated value string into an array of strings
    val dataArray: Array[String] = dataString.split(',')
    var person = 1.0

    if (dataArray(0) == "\"Male\"") {
      person = 0.0
    }

    //Extract the values from the strings
    //Since the data is in US metrics (inch and pounds we will recalculate this to cm and kilo's)
    val data : Array[Double] = Array(person,dataArray(1).toDouble * 2.54)
    val weight: Double = dataArray(2).toDouble * 0.45359237

    //And return the result in a format that can later easily be used to feed to Smile
    return (data, weight)
  }

```

Note that the data reader converts the values from the [Imperial system](http://en.wikipedia.org/wiki/Imperial_units) into the [Metric] system(http://en.wikipedia.org/wiki/Metric_system). This has no big effects on the OLS implementation, but since the metric system is more commonly used, we prefer to present the data in that system.

With these methods we get the data as an ```Array[Array[Double]]``` which represents the datapoints and an ```Array[Double]``` which represents the classifications as male or female. These formats are good for both plotting the data, and for feeding into the machine learning algorithm.

Let's first see what the data looks like. For this we plot the data using the following code.


```scala

object LinearRegressionExample extends SimpleSwingApplication {
  def top = new MainFrame {
    title = "Linear Regression Example"
    val basePath = "/Users/.../OLS_Regression_Example_3.csv"

    val testData = GetDataFromCSV(new File(basePath))

    val plotData = (testData._1 zip testData._2).map(x => Array(x._1(1) ,x._2))
    val maleFemaleLabels = testData._1.map( x=> x(0).toInt)
    val plot =  ScatterPlot.plot(plotData,maleFemaleLabels,'@',Array(Color.blue, Color.green))
    plot.setTitle("Weight and heights for male and females")
    plot.setAxisLabel(0,"Heights")
    plot.setAxisLabel(1,"Weights")



    peer.setContentPane(plot)
    size = new Dimension(400, 400)
  }

```

If you execute the code here above, a window will pop up which shows the **right** plot as shown in the image here below. Note that when you run the code, you can scroll to zoom in and out in the plot.

<img src="./Images/HumanDataPoints.png" width="275px" />
<img src="./Images/MaleFemalePlot.png" width="275px" />


In this plot, given that green is female, and blue is male, you can see that there is a big overlap in their weights and heights. So if we where to ignore the difference between male and female it would still look like there was a linear function in the data (which can be seen in the **left** plot). However when ignoring this difference, the function would be not as accurate as it would be when we incorporate the information regarding males and females. 

Finding this distinction is trivial in this example, but you might encounter datasets where these groups are not so obvious. Making you aware of this this possibility might help you find groups in your data, which can improve the performance of your machine learning application.

Now that we have seen the data and see that indeed we can come up with a linear regression line to fit this data, it is time to train a [model](#model). Smile provides us with the [ordinary least squares](http://en.wikipedia.org/wiki/Ordinary_least_squares) algorithm which we can easily use as follows:

```scala
val olsModel = new OLS(testData._1,testData._2)
```

With this olsModel we can now predict someone's weight based on length and gender as follows: 

```scala
println("Prediction for Male of 1.7M: " +olsModel.predict(Array(0.0,170.0)))
println("Prediction for Female of 1.7M:" + olsModel.predict(Array(1.0,170.0)))
println("Model Error:" + olsModel.error())
```

and this will give the following results:

```
Prediction for Male of 1.7M: 79.14538559840447
Prediction for Female of 1.7M:70.35580395758966
Model Error:4.5423150758157185
```

If you recall from the classification algorithms, there was a [prior](#prior) value to be able to say something about the performance of your model. Since regression is a stronger statistical method, you have an actual error value now. This value represents how far off the fitted regression line is in average, such that you can say that for this model, the prediction for a male of 1.70m is 79.15kg  +/- the error of 4.54kg. Note that if you would remove the distinction between males and females, this error would increase to 5.5428. In other words, adding the distinction between male and female, increases the model accuracy by +/- 1 kg in its predictions.

Finally smile also provides you with some statistical information regarding your model. The method ```RSquared``` gives you the [root-mean-square error (RMSE)](#root-mean-squared-error-rmse) from the model divided by the [RMSE](#root-mean-squared-error-rmse) from the mean. This value will always be between 0 and 1. If your model predicts every datapoint perfectly, RSquared will be 1, and if the model does not perform better than the mean function, the value will be 0. In the field this measure is often multiplied by 100 and then used as representation of how accurate the model is. Because this is a normalised value, it can be used to compare the performance of different models.

This concludes linear regression, if you want to know more about how to apply regression on  non-linear data, feel free to work through the next example [An attempt at rank prediction for top selling books using text regression](#an-attempt-at-rank-prediction-for-top-selling-books-using-text-regression).



###An attempt at rank prediction for top selling books using text regression

In the example of [predicting weights based on heights and gender](#predicting-weight-based-on-height-using-ordinary-least-squares) we introduced the notion of linear regression. However, sometimes one would want to apply regression on non numeric data such as Text. 

In this example we will show how text regression can be done by prediction the top 100 selling books from O'Reilly. Additionally with this example we will also show that for this particular case it does not work out to use text regression. The reason for this is that the data simply does not contain a signal for our test data. However this does not make this example useless because there might be an actual signal within the text in the data you are using in practice,which then can be detected using text regression as explained here.


Let's start off with getting the data we need:

```scala
object TextRegression  {

  def main(args: Array[String]): Unit = {

    //Get the example data
      val basePath = "/users/.../TextRegression_Example_4.csv"
      val testData = GetDataFromCSV(new File(basePath))
  }

  def GetDataFromCSV(file: File) : List[(String,Int,String)]= {
    val reader = CSVReader.open(file)
    val data = reader.all()

    val documents = data.drop(1).map(x => (x(1),x(3)toInt,x(4)))
    return documents
  }
}

```

We now have the title, rank and long description of the top 100 selling books from O'Reilly. However when we want to do regression of some form, we need numeric data. This is why we will build a [Document Term Matrix (DTM)](http://en.wikipedia.org/wiki/Document-term_matrix). Note that this DTM is similar to the Term Document Matrix (TDM) that we built in the spam classification example. It's difference is that we store document records containing which terms are in that document, in contrast to the TDM where we store records of words, containing a list of documents in which this term is available.

We implemented the DTM ourselves as follows:

```scala
import java.io.File
import scala.collection.mutable

class DTM {

  var records: List[DTMRecord] = List[DTMRecord]()
  var wordList: List[String] = List[String]()

  def addDocumentToRecords(documentName: String, rank: Int, documentContent: String) = {
    //Find a record for the document
    val record = records.find(x => x.document == documentName)
    if (record.nonEmpty) {
      throw new Exception("Document already exists in the records")
    }

    var wordRecords = mutable.HashMap[String, Int]()
    val individualWords = documentContent.toLowerCase.split(" ")
    individualWords.foreach { x =>
      val wordRecord = wordRecords.find(y => y._1 == x)
      if (wordRecord.nonEmpty) {
        wordRecords += x -> (wordRecord.get._2 + 1)
      }
      else {
        wordRecords += x -> 1
        wordList = x :: wordList
      }
    }
    records = new DTMRecord(documentName, rank, wordRecords) :: records
  }

  def getStopWords(): List[String] = {
    val source = scala.io.Source.fromFile(new File("/Users/.../stopwords.txt"))("latin1")
    val lines = source.mkString.split("\n")
    source.close()
    return lines.toList
  }

  def getNumericRepresentationForRecords(): (Array[Array[Double]], Array[Double]) = {
    //First filter out all stop words:
    val StopWords = getStopWords()
    wordList = wordList.filter(x => !StopWords.contains(x))
    
    var dtmNumeric = Array[Array[Double]]()
    var ranks = Array[Double]()

    records.foreach { x =>
      //Add the rank to the array of ranks
      ranks = ranks :+ x.rank.toDouble

      //And create an array representing all words and their occurrences for this document:
      var dtmNumericRecord: Array[Double] = Array()
      wordList.foreach { y =>

        val termRecord = x.occurrences.find(z => z._1 == y)
        if (termRecord.nonEmpty) {
          dtmNumericRecord = dtmNumericRecord :+ termRecord.get._2.toDouble
        }
        else {
          dtmNumericRecord = dtmNumericRecord :+ 0.0
        }
      }
      dtmNumeric = dtmNumeric :+ dtmNumericRecord

    }

    return (dtmNumeric, ranks)
  }
}

class DTMRecord(val document : String, val rank : Int, var occurrences :  mutable.HashMap[String,Int] )


```

If you look at this implementation you'll notice that there is a method called  ```def getNumericRepresentationForRecords(): (Array[Array[Double]], Array[Double])```. This method returns a tuple with as first parameter a matrix in which each row represents a document, and each column represents one of the words from the complete vocabulary of the DTM's documents. Note that the doubles in the first table represent the # of occurrences of the words.

The second parameter is an array containing all the ranks beloning to the records from the first table. 

We can now extend our main code such that we get the numeric representation of all the documents as follows:

```scala

val documentTermMatrix  = new DTM()
testData.foreach(x => documentTermMatrix.addDocumentToRecords(x._1,x._2,x._3))

```


With this conversion from text to numeric value's we can open our regression toolbox. We used [Ordinary Least Squares(OLS)](http://en.wikipedia.org/wiki/Ordinary_least_squares) in the example [predicting weight based on height](#predicting-weight-based-on-height-using-ordinary-least-squares), however this time we will use [Least Absolute Shrinkage and Selection Operator (Lasso)](http://en.wikipedia.org/wiki/Least_squares#Lasso_method) regression. This is because we can give this regression method a certain lambda, which represents a penalty value. This penalty value allows the LASSO algorithm to select relevant features while discarding some of the other features. 

This feature selection that Lasso performs is very useful in our case due too the large set of words that is used in the documents descriptions. Lasso will try to come up with an ideal subset of those words as features, where as when applying the OLS, all words would be used, and the runtime would be extremely high. Additionally, the OLS implementation of SMILE detects rank deficiency. This is one of the [curses of dimensionality](#curse-of-dimensionality)

We need to find an optimal lambda however, thus we should try for several lambda using cross validation. We will do this as follows:

```scala

    for (i <- 0 until cv.k) {
      //Split off the training datapoints and classifiers from the dataset
      val dpForTraining = numericDTM._1.zipWithIndex.filter(x => cv.test(i).toList.contains(x._2)).map(y => y._1)
      val classifiersForTraining = numericDTM._2.zipWithIndex.filter(x => cv.test(i).toList.contains(x._2)).map(y => y._1)

      //And the corresponding subset of data points and their classifiers for testing
      val dpForTesting = numericDTM._1.zipWithIndex.filter(x => !cv.test(i).contains(x._2)).map(y => y._1)
      val classifiersForTesting = numericDTM._2.zipWithIndex.filter(x => !cv.test(i).contains(x._2)).map(y => y._1)

      //These are the lambda values we will verify against
      val lambdas: Array[Double] = Array(0.1, 0.25, 0.5, 1.0, 2.0, 5.0)

      lambdas.foreach { x =>
        //Define a new model based on the training data and one of the lambda's
        val model = new LASSO(dpForTraining, classifiersForTraining, x)

        //Compute the RMSE for this model with this lambda
        val results = dpForTesting.map(y => model.predict(y)) zip classifiersForTesting
        val RMSE = Math.sqrt(results.map(x => Math.pow(x._1 - x._2, 2)).sum / results.length)
        println("Lambda: " + x + " RMSE: " + RMSE)

      }
    }
    
```

Running this code multiple times gives an RMSE varying between 36 and 51. This means that the rank prediction we would do would be off by at least 36 ranks. Given the fact that we are trying to predict the top 100 ranks, it shows that the algorithm performs very poorly. Differences in lambda are in this case not noticeable. However when using this in practice you should be careful when picking the lambda value: The higher the lambda you pick, the lower the amount of features for the algorithm becomes. This is why cross validation is important to see how the algorithm performs on different lambda's.

To conclude this example, we rephrase a quote from [John Tukey](http://en.wikipedia.org/wiki/John_Tukey): 
>The data may not contain the answer. The combination of some data and an aching desire for an answer does not ensure that a reasonable answer can be extracted from a given body of data.



###Using unsupervised learning to merge features (PCA)

In this example we are going to use [PCA](#principal-components-analysis-pca) to merge the stock prices from 24 stocks into 1. This single value then represents a stock market index based on data of these 24 stocks. We will use data from 2002 until 2012. Merging these 24 features into 1 significantly reduces the amount of data to process, and decreases the dimension of our problem, which is a big advantage if we later apply other machine learning algorithms such as regression for prediction.

Note that reducing the dimension from 24 to 1 causes 'data loss'. However, because the stock prices are correlated, we accept this 'data loss'. When applying this method yourself, you should verify first whether your data is correlated.

Let's start the example

TODO: create the final code bits and write the section



###using Support Vector Machines

In this example we will work through several small cases where a Support Vector Machine (SVM) will outperform other classifying algorithms such as [KNN](#labeling-isps-based-on-their-downupload-speed-knn-using-smile-in-scala). This approach is different from the former examples, but will help you understand how and when to use SVM's more easily. 

For each sub example we will provide code, a plot, a few test runs with different parameters on the SVM and an explanation on the results. This should give you an idea on the parameters to feed into the SVM algorithm. 

In the first example we will use the GaussianKernel, but there are many other kernels available in [Smile](https://github.com/haifengl/smile). The other kernels can be found [here](http://haifengl.github.io/smile/doc/smile/math/kernel/MercerKernel.html), and some of them we will use in the other examples.


 We will use the following base for each example, with only the ```path``` and ```svm``` construction changing per example.

```scala

object SupportVectorMachine extends SimpleSwingApplication {


  def top = new MainFrame {
    title = "SVM Examples"
    //File path (this changes per example)
    val path =  "/users/.../Example Data/SVM_Example_1.csv"

    //Loading of the test data and plot generation stays the same
    val testData = GetDataFromCSV(new File(path))
    val plot = ScatterPlot.plot(testData._1, testData._2, '@', Array(Color.blue, Color.green))
    peer.setContentPane(plot)

    //Here we do our SVM fine tuning with possibly different kernels
    val svm = new SVM[Array[Double]](new GaussianKernel(0.01), 1.0,2)
    svm.learn(testData._1, testData._2)
    svm.finish()

    //Calculate how well the SVM predicts on the training set
    val predictions = testData._1.map(x => svm.predict(x)).zip(testData._2)
    val falsePredictions = predictions.map(x => if (x._1 == x._2) 0 else 1 )

    println(falsePredictions.sum.toDouble / predictions.length  * 100 + " % false predicted")

    size = new Dimension(400, 400)
  }


  def GetDataFromCSV(file: File): (Array[Array[Double]], Array[Int]) = {
    val source = scala.io.Source.fromFile(file)
    val data = source.getLines().drop(1).map(x => GetDataFromString(x)).toArray
    source.close()
    val dataPoints = data.map(x => x._1)
    val classifierArray = data.map(x => x._2)
    return (dataPoints, classifierArray)
  }

  def GetDataFromString(dataString: String): (Array[Double], Int) = {
    //Split the comma separated value string into an array of strings
    val dataArray: Array[String] = dataString.split(',')

    //Extract the values from the strings
    val coordinates  = Array( dataArray(0).toDouble, dataArray(1).toDouble)
    val classifier: Int = dataArray(2).toInt

    //And return the result in a format that can later easily be used to feed to Smile
    return (coordinates, classifier)
  }
  
```

So now to start with the first example. If we load the first example we get to see the following data plot:

<img src="./Images/SVM_Datapoints.png" width="400px" />

It is clear from this plot that a linear regression line would not work. Instead we will use a SVM to make predictions. In the first code we gave, the GaussianKernel with a sigma of 0.01, a margin penalty of 1.0 and the amount of classes of 2 is passed to SVM. Now what does this all mean?

Lets start with the  ```GaussianKernel```. This kernel represents the way in which the SVM will calculate the similarity over pairs of datapoints in the system. For the gaussianKernel the variance in the euclidian distance is used. The reason for picking the GaussianKernel specifically is because the data does not contain a clear structure such as a linear, polynomial or hyperbolic function. Instead the data is clustered in 3 groups. 

The parameter we pass in the constructor of the ```GaussianKernel``` is the sigma. This sigma value represents a smoothness value of the kernel. We will show what changing this parameter has for effect in the predictions.  As margin penalty we pass 1. This parameter defines the margin of the vectors in the system, thus making this value lower results in more bounded vectors. Again we will show with runs what kind of effect this has in practice:

| | s: 0.001 | s: 0.01 | s: 0.1 | s: 0.2 | s: 0.5 | s: 1.0 | s: 2.0 | s: 3.0 | s: 10.0 | s: 100.0 |
| :-- | :--: | :--: | :--: | :--: | :--: | :--: | :--: |  :--: |  :--: |  :--: | 
| **c: 0.001** | 48.4% | 48.4% | 48.4% | 48.4% | 48.4% | 48.4% | 48.4% | 48.4% | 48.4% | 48.4% |
| **c: 0.01** | 48.4% | 48.4% | 40% | 43.8% | 48.4% | 48.4% | 48.4% | 48.4% | 48.4% | 48.4% |
| **c: 0.1** | 48.4% | 48.4% | 12.4% | 14.2% | 17.4% | 48.4% | 48.4% | 48.4% | 48.4% | 48.4% |
| **c: 0.2** | 48.4% | 45.6% | 9.1% | 10.1% | 12.3% | 48.4% | 48.4% | 48.4% | 48.4% | 48.4% |
| **c: 0.5** | 47.5% | 3.4% | 6.3% | 7.2% | 7% | 8.9% | 48.4% | 48.4% | 48.4% | 48.4% |
| **c: 1.0** | 0% | 1.6% | 5.1% | 5.7% | 5.6% | 5.6% | 48.4% | 48.4% | 48.4% | 48.4% |
| **c: 2.0** | 0% | 1% | 5.2% | 5% | 5.4% | 5.7% | 13.1% | 48.4% | 51.6% | 48.4% |
| **c: 3.0** | 0% | 1.2% | 6.4% | 5.8% | 5.7% | 7.4% | 18% | 51.6% | 51.6% | 51.6% |
| **c: 10.0** | 0% | 1.5% | 7.5% | 6.4% | 7.7% | 12.9% | 26.2% | 51.6% | 51.6% | 51.6% |
| **c: 100.0** | 0% | 1.5% | 10.1% | 12.8% | 14.6% | 18.3% | 41.6% | 51.6% | 51.6% | 51.6% |
