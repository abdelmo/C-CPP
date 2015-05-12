/* Solution for Dinig Philosophers problem implemented for VxWorks for ARM */

/*************************************************************************/
/*  HelloSemaPhore.c                                                     */
/*                                                                       */
/*************************************************************************/


/* includes */
#include "vxWorks.h"
#include "stdio.h"
#include "stdlib.h"
#include "semLib.h"
#include "taskLib.h"

/* defines */
#define DELAY_TICKS  60
#define STACK_SIZE   20000
#define MAXSECONDS   100
#define MAXPHILO   20


int i;

int *p ;

/* Semaphore IDs */
SEM_ID *Sema_fork;	

int *tPhil;

/* task IDs */


int X;


/* function declarations */
void Philosopher (int,int);




/*************************************************************************/
/*  main task                                                            */
/*                                                                       */
/*************************************************************************/

int main (void) {
		
	int nseconds = 0;
	int Philos = 0;
	X=0;
	
	/* get the simulation time */ 
	while ((nseconds<1) || (nseconds > MAXSECONDS)) {
		printf ("Enter overall simulation time [1-%d s]: ", MAXSECONDS);
		scanf ("%d", &nseconds);
    };
    
    
    while ((Philos<1) || (Philos > MAXSECONDS)) { 
	printf ("Enter Number of Philosophers [3-%d]: ", MAXPHILO);
	scanf ("%d", &Philos);
    };
    while ((X<1) || (X > MAXSECONDS)) {
	printf ("Enter waiting time [3-%d ticks]: ", MAXSECONDS);
	scanf ("%d", &X);
    };

	printf("\nSimulating %d philosophers with waiting time %d ticks for %d seconds...\n\n", Philos, X, nseconds);
	
	
	p = (int*)malloc( Philos * sizeof(int) );
		
		for (i=0 ; i<Philos ; i++){
			p[i]=0;
		}
	
	Sema_fork = (SEM_ID*)malloc( Philos * sizeof(SEM_ID) );

	/* create binary semaphores */
	for (i=0 ; i<Philos ; i++){
	Sema_fork[i]  = semBCreate (SEM_Q_FIFO, SEM_FULL);
	}
	
    
	/* spawn (create and start) tasks */
	tPhil = (int*)malloc( Philos * sizeof(int) );
	for (i=0 ; i<Philos ; i++){
	tPhil[i] = taskSpawn ("tPhil", 200, 0, STACK_SIZE, 
		(FUNCPTR) Philosopher, i,Philos,0,0,0,0,0,0,0,0);
	}
	


	/* run for the given simulation time */
	taskDelay (nseconds*60);
	
	/* delete tasks and semaphores */
	for (i=0 ; i<Philos ; i++){
	taskDelete (tPhil[i]);
	semDelete (Sema_fork[i]);
	printf("\n\n\n");
	printf("Philosopher %d ate %d times\n" , i+1 , p[i]);	
	}
		
	printf("\n\n All philosophers stopped.\n");
	return(0);
}   



/*************************************************************************/
/*  task "professor 1"                                                        */
/*                                                                       */
/*************************************************************************/

void Philosopher (int i, int Philos)   {
	while (1) {

             if (i==Philos-1){
 				taskDelay (DELAY_TICKS);
 				printf("Philosopher %d - start thinking\n", i+1);
 				semTake(Sema_fork[0], WAIT_FOREVER);
 				taskDelay(X);
 				semTake (Sema_fork[Philos-1], WAIT_FOREVER);
 				printf("Philosopher %d - start eating\n", i+1);
 				taskDelay(10);
 				semGive(Sema_fork[Philos-1]);
 				semGive(Sema_fork[0]);
                p[i]++;
            	           	 
             }
             else {
				taskDelay (DELAY_TICKS);
				printf("Philosopher %d - start thinking\n", i+1);
				semTake(Sema_fork[i], WAIT_FOREVER);
				taskDelay(X);
				semTake (Sema_fork[i+1], WAIT_FOREVER);
				printf("Philosopher %d - start eating\n", i+1);
				taskDelay(10);
				semGive(Sema_fork[i]);
				semGive(Sema_fork[i+1]);
                p[i]++;
              }		
	};
	
}
