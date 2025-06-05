Notes from Stanford CS229 Lecture 16: Independent Component Analysis<br>
-Trying to separate "j" sources at time steps 0 to i, given that they are mixed linearly. <br>
-The data needs to be non-Gaussian, because Gaussian data is rotationally ambiguous (symmetric). The data, when graphed, has circular contours, which is not useful for us, whereas Non-Gaussian data represents a rectangular or parallelogram shape which can be returned to the original square by ICA.<br>
-ICA output doesn't know which speaker is which, and the data can be flipped/reflected.<br>
-Gaussian density function is the only density function that is rotationally symmetric, so ICA only possible if data is non-Gaussian.<br>

To develop the algorithm, we need to figure out the density of S, which can be done via the CDF (cumulative density function) of S, because the CDF is the integral of the PDF.<br>
Instead of specifying a PDF, we need to specify a CDF. 
<br>
x = As = (W^-1)s<br>
s=Wx<br>
This is our model for the relationships between s and x. What is the relationship between the densities of X and S? We cannot just say they are equal, d(x) does not equal d(Wx), because the matrix multiplication changes the density function.<br>
We need to scale the equation by the determinant of the matrix W. Thus, d(x) = d(Wx)*det(W). <br>
The last thing that we need to set up the model is to choose the density of s, the speaker's voices. <br>
We need a smooth function for the CDF, so we can choose the sigmoid function. This function yields a PDF that is almost a fatter-tailed Gaussian.
Double exponential distribution (Laplacian) would work as well.<br>

The density of s is thus equal to the product from 1 to n sources of the probability of each of the speakers emitting that sound, because each of the speakers are independent, and thus we must just multiply the individual probabilities to obtain the joint probability.<br>
So, the density of x is the density of s times det(W), which is equal to the product of the individual probabilities p(Wj^T*x) * det(W). This middle term is basically just the probability of the individual s, which is the x vector times the transpose of the jth row of W.<br>
Thus, given a square matrix W, we can write the density of X. 
<br>
Now, we use maximum likelihood estimation to estimate W. 
The log-likelihood of W is equal to the sum of the logs of the sigmoid densities of x. <br>
We can use SGD,
<img width="464" alt="Screenshot 2025-06-05 at 9 15 27 AM" src="https://github.com/user-attachments/assets/0e8f2210-a79f-458d-aa62-1fa85c08cb93" />


Use this derivative wrt W, using SGD to maximize the likelihood, then we can find a good matrix W. 
