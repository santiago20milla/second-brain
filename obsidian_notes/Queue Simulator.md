# Queue Simulator
#python #code_snippet #queueing_theory 

```
import simpy
import pandas as pd
import random
import numpy as np
from functools import partial, wraps
import os
from collections import deque

random_seed = 42
num_servers = 1
arrival_rate = 50
sim_time = 3*60
service_time = 20 # mean
service_time_std = 5 # sigma
service_time_min = 5 # min

queue = deque()
customer_log = []

class QueueServer: # simulation agents
    
	def __init__(self, env, num_servers):
        self.env = env
		self.queue_server = simpy.Resource(env, num_servers)
		
	def do_process(self, abandon):
	    if abandon:
		    yield self.env.timeout(0)
		else:
		    yield self.env.timeout(max(
			    service_time_min,
				random.normalvariate(service_time, service_time_std)
				))
				
def QueueCustomer(env, customer_id: int, queue_server):
    enqueue_time = env.now
	print('Customer %d enters Queue %.2f.' % (customer_id, enqueue_time))
	queue.appendleft(customer_id)
	
	with queue_server.queue_server.request() as request:
	    yield request
		start_time = env.now
		print('Customer %d enters Server %.2f.' % (customer_id, start_time))
		queue.pop()
		
		queue_time = start_time - enqueue_time
		abandon = is_impatient(queue_time, random.random())
		
		yield env.process(queue_server.do_process(abandon))
		finish_time = env.now
		print('Customer %d exits Server %.2f.' % (customer_id, finish_time))
		
		# save data
		customer_log.append((
		    customer_id,
			enqueue_time,
			start_time,
			finish_time,
			len(queue),
			abandon
			))
			
# simulation functions
def setup(env, num_servers, service_time, arrival_rate):
    queue_server = QueueServer(env, num_servers)
	
	i = 0
	env.process(QueueCustomer(env, i, queue_server))
	while True:
	    yield env.timeout(random.expovariate(1/arrival_rate))
		i +=1
		env.process(QueueCustomer(env, i, queue_server))
		
def is_impatient(queue_time, random_number, queue_time_thresh=5, impatient_prob=0.2):
    if queue_time > queue_time_thresh and random_number > impatient_prob:
	    return True
	else:
	    return False
		

random.seed(random_seed)
env = simpy.Environment()

env.process(setup(env, num_servers, service_time, arrival_rate))
env.run(until=sim_time)

customer_df = pd.DataFrame(
    customer_log,
	columns=[
	    'customer_id',
	    'enqueue_time',
		'start_time',
		'finish_time',
		'queue_length_upon_exit',
		'is_abandoned'
		]
)

customer_df['queue_time'] = customer_df['start_time'] - customer_df['enqueue_time']
print(customer_df)
```