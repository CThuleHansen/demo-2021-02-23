# Error Handling Disabled
```c
simulation 
import FMI2;
import MEnv;
@Framework( "FMI2");
{
    bool globalExecutionContinue = true;
  	int fmi_status_discard = 2;
  	int fmi_status_ok = 0;
  	int status;
  	while( globalExecutionContinue )
  	{
  		MEnv menv = load("MEnv");
  		FMI2 controllerfmu = load("FMI2", "{8c4e810f-3df3-4a00-8276-176fa3c9f000}", "/Users/au443759/source/into-cps-association/maestro/maestro/src/test/resources/watertankcontroller-c.fmu");
  		FMI2 tankfmu = load("FMI2", "{cfc65592-9ece-4563-9705-1581b6e7071c}", "/Users/au443759/source/into-cps-association/maestro/maestro/src/test/resources/singlewatertank-20sim.fmu");
  		FMI2Component controller = controllerfmu.instantiate("controller", true, true);
  		bool controllerCurrentTimeFullStep = true;
  		real controllerCurrentTime = 0.0;
  		bool controllerBoolShare[1];
  		real controllerRealIo[4] = { 0.0 , 0.0 , 0.0 , 0.0 };
  		bool controllerBoolIo[4] = { false , false , false , false };
  		uint controllerUintVref[4] = { 0 , 0 , 0 , 0 };
  		FMI2Component tank = tankfmu.instantiate("tank", true, true);
  		bool tankCurrentTimeFullStep = true;
  		real tankCurrentTime = 0.0;
  		real tankRealShare[1];
  		real tankRealIo[22] = { 0.0 , 0.0 , 0.0 , 0.0 , 0.0 , 0.0 , 0.0 , 0.0 , 0.0 , 0.0 , 0.0 , 0.0 , 0.0 , 0.0 , 0.0 , 0.0 , 0.0 , 0.0 , 0.0 , 0.0 , 0.0 , 0.0 };
  		uint tankUintVref[22] = { 0 , 0 , 0 , 0 , 0 , 0 , 0 , 0 , 0 , 0 , 0 , 0 , 0 , 0 , 0 , 0 , 0 , 0 , 0 , 0 , 0 , 0 };
  		tankUintVref[0] = 17;
  		tankRealIo[0] = 0.0;
  		status = tank.setReal(tankUintVref, 1, tankRealIo);
  		controllerUintVref[0] = 4;
  		controllerBoolIo[0] = false;
  		status = controller.setBoolean(controllerUintVref, 1, controllerBoolIo);
  		int tmp = menv.getInt("lowerlevel");
  		controllerUintVref[0] = 1;
  		controllerRealIo[0] = tmp;
  		status = controller.setReal(controllerUintVref, 1, controllerRealIo);
  		real tmp1 = menv.getReal("upperlevel");
  		controllerUintVref[0] = 0;
  		controllerRealIo[0] = tmp1;
  		status = controller.setReal(controllerUintVref, 1, controllerRealIo);
  		status = tank.setupExperiment(false, 0.0, 0.0, true, 11.0);
  		status = controller.setupExperiment(false, 0.0, 0.0, true, 11.0);
  		status = tank.enterInitializationMode();
  		status = controller.enterInitializationMode();
  		tankUintVref[0] = 17;
  		status = tank.getReal(tankUintVref, 1, tankRealIo);
  		tankRealShare[0] = tankRealIo[0];
  		controllerUintVref[0] = 4;
  		status = controller.getBoolean(controllerUintVref, 1, controllerBoolIo);
  		controllerBoolShare[0] = controllerBoolIo[0];
  		status = tank.exitInitializationMode();
  		status = controller.exitInitializationMode();
  		real time = 0.0;
  		real step_size = 0.1;
  		while( time + step_size < 10.0 )
  		{
  			tankUintVref[0] = 16;
  			if( controllerBoolShare[0] )
  			{
  					tankRealIo[0] = 1.0;
  			}
  			else
  			{
  					tankRealIo[0] = 0.0;
  			}
  			status = tank.setReal(tankUintVref, 1, tankRealIo);
  			controllerUintVref[0] = 3;
  			controllerRealIo[0] = tankRealShare[0];
  			status = controller.setReal(controllerUintVref, 1, controllerRealIo);
  			status = tank.doStep(time, step_size, false);
  			if( status != fmi_status_ok )
  			{
  				if( status == fmi_status_discard )
  				{
  						status = tank.getRealStatus(2, ref tankCurrentTime);
  						tankCurrentTimeFullStep = false;
  				}
  			}
  			else
  			{
  					tankCurrentTime = time + step_size;
  					tankCurrentTimeFullStep = true;
  			}
  			status = controller.doStep(time, step_size, false);
  			if( status != fmi_status_ok )
  			{
  				if( status == fmi_status_discard )
  				{
  						status = controller.getRealStatus(2, ref controllerCurrentTime);
  						controllerCurrentTimeFullStep = false;
  				}
  			}
  			else
  			{
  					controllerCurrentTime = time + step_size;
  					controllerCurrentTimeFullStep = true;
  			}
  			tankUintVref[0] = 17;
			status = tank.getReal(tankUintVref, 1, tankRealIo);
			tankRealShare[0] = tankRealIo[0];
			controllerUintVref[0] = 4;
			status = controller.getBoolean(controllerUintVref, 1, controllerBoolIo);
			controllerBoolShare[0] = controllerBoolIo[0];
			time = time + step_size;
		}
		if( controllerfmu != null )
		{
				unload(controllerfmu);
				controllerfmu = null;
		}
		if( tankfmu != null )
		{
				unload(tankfmu);
				tankfmu = null;
		}
		if( menv != null )
		{
				unload(menv);
		}
		break;
	}
}
```
