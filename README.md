


#  FreeRTOS系统的开发二



## 项目内容

项目主要是通过cubemx生成的FreeRTOS的一个CMSISv2版本的模板，其中我添加了两个任务为task1、task2。
在上个项目的基础上进行任务挂起、继续运行控制的研究，其中运用到四个函数：osThreadSuspend、vTaskSuspend、osThreadResume、vTaskResume。其中Suspend是挂起任务，有点类似ios的后台挂机的机制，Resume则是重新回到任务中。

## Task1（没有大修改）

		for(;;)
		  {
			  HAL_GPIO_TogglePin(GPIOD, GPIO_PIN_2);
			  osDelay(500);
		  }
我把ss的变量自加改到Task2中，其中的原因就是运用osThreadSuspend函数会清空任务中的全局变量参数，根据手册提示使用vTaskSuspend函数会保存变量的参数，对于此问题我并没有过多的研究。
## Task2（修改为任务控制）

		for(;;)
		  {
			 ss++;
			 if(ss==4)
			 {
				 printf("hello!%d\r\n",ss);
				 //osThreadSuspend(Task1Handle);    //会清空任务中的 全局数据
				 vTaskSuspend(Task1Handle);			//会清空任务中的 全局数据
			 }
			 else if (ss==10)
			 {
				 //osThreadResume(Task1Handle);		//会清空任务中的 全局数据
				 vTaskResume(Task1Handle);			//不会清空任务中的 全局数据
				 ss=0;
			 }
			 osDelay(500);
		  }
我在任务2是对于任务1中的LED闪烁进行任务挂机、继续运行。


## 留言：
本期主要是对于上一个项目的几个bug进行研究，以及对于后面的研究内容进行资料查询，因此并没有过多的资料、



# BY：小十一
