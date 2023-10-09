# Traffic handling

### Current Status

As our collaboration with Nike continues, they recently e-mailed us regarding an increase in traffic to the application. They have requested to ensure reliability with a minimum of 14,000 user access at any given point.

### Observations

We initiated network traffic testing by stressing the t2.medium instance, which had been upgraded from the previous t2.micro configuration. During the stress test, we spawned 14,000 threads to access the application to establish a baseline before implementing any optimizations.

Following the test, we observed that 56 (.4%) users were unable to access the application.

### Resolution

Several optimization strategies could relieve networking traffic. The application could be scaled vertically. AWS offers numerous different types of EC2 instances catered to specific needs. After considering costs and Nike’s request for 14,000 minimum user access, a t3a.xlarge was selected. Although upgrading to t2.xlarge or t2.2xlarge might resolve the issue, t3a offers better features to our specific business model. T3a instances are able to handle traffic better through the use of next-generation burstable, with the ability to burst CPU usage at any time for as long as required. They have also included both EBS and networking enhancements with this tier, all while saving Nike 10% over t2 instances.

### Optimization

- The possibility of deploying the application with EBS to fully take advantage of the instances enhancements (EBS-optimized instances deliver dedicated throughput between Amazon EC2 and Amazon EBS, with options between 500 Megabits per second (Mbps) and 80 Gigabits per second (Gbps), depending on the instance type used.)

- Horizontal scaling with the implementation of a load balancer could also improve traffic handling while lowering cost (1 t3a.xlarge .01504 per hr vs 2 t2.medium .0928 per hr). The disadvantage is that it would have less RAM, however, while monitoring RAM usage, 4GB would still be able to handle the workload as it never surpassed 25%

### Results
- Before Optimization: CPU 1,2 both reached 99% usage
- After Vertical Scaling: CPU 1,2,3,4 has decreased usage by 20-30% 
- Considering the amount of traffic that wasn’t able to reach the application (.4% or 56/14,000), doubling resources should be more than enough to handle the surge in traffic at any given time

### Conclusion
In conclusion, to enhance traffic handling for the application, Vertical scaling was used. AWS offers a large range of EC2 instances tailored to specific needs. After a careful evaluation of costs and Nike's minimum user access requirement of 14,000, the t3a.xlarge instance was selected as the optimal choice. 

- **Case usage for t3a EC2: Micro-services, low-latency interactive applications, small and medium databases, virtual desktops, development environments, code repositories, and business-critical applications

- **T3a instances offer a balance of compute, memory, and network resources and are designed for applications with moderate CPU usage that experience temporary spikes in use. T3a instances deliver up to 10% cost savings over comparable instance types.
Resource: https://aws.amazon.com/ec2/instance-types/ 
