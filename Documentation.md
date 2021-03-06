## Objective

To design domain specific langauge for Classification algorithms and implementing a lexical analyzer for it.
Language designed below specifies constructs, operations and operators required for classification. 

## Application Domain

Language is specific to Classification Algorithms (Neural Networks, Naïve Bayes, Linear regression with gradient descent 
and K Nearest neighbors)

## Helpful Features

### Array Operations

Addition, subtraction, multiplication, increment, &, | etc. operations that are supported on integers 
will be supported on integer arrays. Such operations will be applied element wise to the arrays. For two arrays, 
it will be applied elements with equal indexes in respective arrays but will be supported on if array sizes are equal. 
This will be useful for data science as we need to apply many operations on matrices.
   
                          int arr1[10][10], arr2[10][10];
                          arr1[][]++; //add 1 to all elements of arr1
                          arr2[][] – arr1[][]; /* subtract elements of arr1 from arr2 at same index */
                              
### Mathematical Functions
    
##### Sigma function
This is used for summation.

                           sigma(iterating variable, start value, end value){
                              //formula that has to be summed
                           }
                           
##### Sigmoid function
This function accepts a variable and return sigmoidal value

                           double sigmoid(value){
                              return 1/(exponent(-value)+1);
                           }
                           
##### Exponent function
Return e^argument.

                           double exponent(argument){
                              return math.exp(argument)
                           }
                           
### Neural Networks

##### Model attributes
      - Layers vector (add layer) – can be iterated using for (shown in example)
      - Learning rate
      - Input layer, Output layer

##### Layer attributes
   
      - Layers[layer name][node number]
      - Node vector – can be iterated using for (shown in example)
      - addNode, numNodes
      - layerName - string
      
##### Node attributes   
   
      - Weights- dictionary {<layer name, node number> : weight, <layer name, node number> : weight}
      - Step function
      - Threshold

##### TrainModel, TestModel, Classify
      
```
                           classificationModel<ANN> myNet;
                           myNet.inputLayer=‟input‟;
                           myNet.outputLayer=‟output‟;
                           myNet.learningRate=0.4;
                           myNet.layer['input'].addNodes(4); //adds 4 nodes to layer named input
                           myNet.layer['output'].addNodes(1);
                           myNet.layer['hidden'].addNodes(2);
                           //layer['foo'] will create a new layer named foo if not present
                           myNet.layer['hidden'][1].weights = { <‟input‟,1>: 0.5, <‟input‟,2>: 0.5 };
                           myNet.layer['hidden'][2].weights = { <‟input‟,3>: 0.5, <‟input‟,4>: 0.5 };
                           myNet.layer['output'][1].weights = { <‟hidden‟,1>: 0.5, <‟hidden‟,2>: 0.5 };
                           
                           for level in myNet.layer{
                              for node in myNet.layer(level){
                                 myNet.layer['hidden'][i].stepFunction = "sigmoid";
                                 myNet.layer['hidden'][i].threshold = 0.46;
                              }
                           }
                           
                           myNet.trainModel("neuralTrainingData.csv");
                           testResults<ANN> annres= myNet.testModel("neuralTestingData.csv");
                           printf('ANN model test results')
                           vector<int> sample = {3.5, 2.47452, 0.004112, 124}
                           int result = myNet.classify(sample);
                           vector<int> irisResults = myNet.classifyFromFile("irisData.csv");
                           
```                          
                           
### Regression Using Gradient Descent

##### Model attributes
   
      - Hypothesis coefficient vector
      - Cost function, Error
      - X and Y vectors – the input and target vectors

                           classificationModel<RGD> regression;
                           regression.hypothesissize = 3; // hypothesis of the form a0 + a1x + a2x2
                           regression.costFunction = sigma(i,1,regression.hypothesissize){ 
                                                         pow(regression.X[i]–regression.Y[i],2)
                                                     };
                           untilConverge(regression.error = 0.1){
                              regression.trainModel("regressionTrainingData.txt");
                           }
                           testResults<RGD> testing= regression.testModel('regressionTestingData.txt');
                           print(testing);
                           vector<double> result = regression.classifyFromFile('propertyValues.csv');
                           printf('Property Value results:\n' + result);

### KNN

##### Model functions
   
      - createModel(string filename)
      - testResults<knn> testModel(string filename, int k_value) – testResults.error is updated
      - vector<string> classify(string filename)
      - setDistanceParameter(double parameter) – parameter of 1 corresponds to Hamiltonian distance, 2 corresponds to 
        Euclidean distance and so on
      - vector<string> confidenceCalculation(vector point) – 
        vector returned contains class names in decreasing order of confidence with respect to data point 
        calculated by default approach. For custom confidence calculation, set the confidence logic code 
        in KNN model is as follows:

                           knn_model.confidenceCalculation(vector data_point) = {
                              //logic for Confidence Calculation
                           }
##### for Construct
              
                           for point in km.nearestNeighbours(new_point){ //Statements }

##### Model attributes
   
      - Distance parameter

               classificationModel<KNN> knn;
               knn.createModel('iris.txt');
               knn.setDistanceParameter(3);
               int threshold = 4;
               vector<int> new_point = (2,3,6,1,0);
               vector<string> class_names;
               for point in knn.nearestNeighbours(new_point){
                  /*dist() is used for distance calculation between 2 vectors based on distance parameter */
                  if(knn.dist(new_point, point) < threshold){
                     class_names.add(point[-1]);
                  }
                  else{
                     break;
                  }
               }
               //majority returns the terms with highest frequency
               class_names = majority(class_names);
               if(size(class_names) == 1){
                  printf(“Class name: ”+class_names[0]);
               }
               else{
               /* Confidence calculation by choosing class which contains a point closest to data point */
                  knn.confidenceCalculation(vector data_point) = {
                     for point in knn.nearestNeighbours(new_point){
                        if(point[-1] in class_names){
                           break;
                        }
                     }
                     return point[-1];
                  }
               }
               
### Naive Bayes

##### Model Attributes
   
      - priorProbabilities – dictionary containing classname as key and prior Probabilities as value
      
##### Model Functions
   
      - trainModel(string filename, vector<string> classnames)
      - testResults<naiveBayes> testModel(filename) - testResults.error updated.
      - vector<string> classify(filename)
      
##### Constructs
   
      - Probability calculations are dependent on whether attribute is discrete or continuous.
      - Custom class conditional probability is set as follows:
      
               nbc.defaultProbabilityCalculationDiscrete (vector<string> attribute, Boolean change_value)
               /* 
                  [‘all’] is specified for all the attributes to follow new probability calculations 
                  else specified vector of strings follow new probability calculations
               */   
      
               nbc.discreteProbabilityCalculation(int desirable_outcomes, int total_outcomes) = {
               //new probability estimation
               }
      
               /*
                  Consider the training data set with input fields Wind speed, Power output, Generator 
                  Winding Temperature and output field Wind Turbine status as specified in wind.txt. Unclassified 
                  data points are specified in classification.txt file. Usage of Naïve Bayes classifier is as follows
               */   

               classificationModel<naiveBayes> nbc;
               nbc.trainModel("wind.txt");
               nbc.defaultProbabilityCalculationDiscrete("Power output", false);
               nbc.discreteProbabilityCalculations(int desirable_outcomes, int total_outcomes) = {
                  int m = 2; //setting m-estimate parameter
                  double p = (desirable_outcomes / total_outcomes);
                  return (desirable_outcomes + m*p) / (total_outcomes + m);
               }
               result = nbc.classify(„testing.txt‟);
               printf(“Class of result is: ”+result);
               
### Non-Blocking Assignments and Assignment Blocks

Non-blocking assignment like the ‘<=’ in Verilog can be helpful for simultaneous update of multiple items without regard to order or dependence upon on each other. Once example of where it will be useful is when we update parameters of cost-function using gradient descent.

               //Non-blocking assignment
               a := b;
               b := a; //this will swap a and b
               nonBlocking{
               //j is array of coefficients of cost function
               j[0] = j[0] – diff(j[],0);
               j[1] = j[1] – diff(j[],1);
               j[2] = j[2] - diff(j[],2);
               j[3] = j[3] – diff(j[],3);
               }//all four updates happen in a non-blocking manner
               
##### dataContainer data type

This is a data type that will act as a container for data used for data science tasks. It is  similar to how C++ defines containers like Vector, Stack and Queue of types int, char etc. e. g. Vector<int> or Stack<char>. The types accepted by the data container will be image, csv, xls. Type image will allow pixel by pixel traversal, useful for image processing purposes. Type csv and xls can be used for reading in data from csv and excel files and used for traversing through data points, used for getting field names and manipulating data at a lower level.

               //creating a data container of image type
               dataContainer<image> img= loadImage(“~/pictures/iris.jpg”);
               
##### Database Functions 

Connection to database can be established easily and queries run on it directly with simple syntax. Currently, with the rise of big data, there is increasing need for running machine learning and AI algorithms on data easily selected through joins/views from an efficient database system. This feature will decrease hassle of any programmer trying to work with both data from a database and ML algorithms.

               //creating a database variable with connection details
               database db = connect(“user_name”,”sales_db”,”localhost”);
               int max = db(“select max(sales) from shoe_sales”);

##### HTTP Requests

Using get, put, post and delete HTTP requests, we can easily interact with data on servers. Very useful for interacting with cloud based data storage services. This removes the limitation of a data science programmer to interact and work with data on a local machine.

               put(“http::/server/script/resources?query”);

