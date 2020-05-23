# Change-default-kernel-scheduler
Modifications to Kernel source files to force change the default kernel scheduler from Completely Fair Scheduler to FIFO scheduler

This is for Kernel Version 5.6.14

Download the kernel from kernel.org and navigate to kernel->sched

Replace the core file with this file.

The sched_fork file was modified with these lines:

 if (p->policy == SCHED_NORMAL) {
         p->prio = current->normal_prio - NICE_WIDTH - PRIO_TO_NICE(current->static_prio);
         p->normal_prio = p->prio;
         p->rt_priority = p->prio;
         p->policy = SCHED_FIFO;
         p->static_prio = NICE_TO_PRIO(0);
     }

The _sched_setscheduler was modified with these lines :
 if (attr.sched_policy == SCHED_NORMAL) {
         attr.sched_priority = param->sched_priority - NICE_WIDTH - attr.sched_nice;
         attr.sched_policy = SCHED_FIFO;
     }

Follow the procedure here to update kernel after modifications : https://www.cyberciti.biz/tips/compiling-linux-kernel-26.html


Credits to Lorenzo Nava
