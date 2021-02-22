# Error Handling Enabled
```c
simulation 
  import FMI2;
  import Logger;
  import MEnv;
  @Framework( "FMI2");
  {
    int fmi_status_ok = 0;
  	int fmi_status_warning = 1;
  	int fmi_status_discard = 2;
  	int fmi_status_error = 3;
  	int fmi_status_fatal = 4;
  	int fmi_status_pending = 5;
  	bool globalExecutionContinue = true;
  	int status;
  	while( globalExecutionContinue )
  	{
  		MEnv menv = load("MEnv");
  		Logger logger = load("Logger");
  		FMI2 controllerfmu = load("FMI2", "{8c4e810f-3df3-4a00-8276-176fa3c9f000}", "/Users/au443759/source/into-cps-association/maestro/maestro/src/test/resources/watertankcontroller-c.fmu");
  		if( controllerfmu == null )
  		{
  				globalExecutionContinue = false;
  				logger.log(4, "FMU load failed on fmu: '%s' for uri: '%s'", "controllerFmu", "file:///Users/au443759/source/into-cps-association/maestro/maestro/src/test/resources/watertankcontroller-c.fmu");
  				unload(logger);
  				logger = null;
  				unload(controllerfmu);
  				controllerfmu = null;
  				break;
  		}
  		FMI2 tankfmu = load("FMI2", "{cfc65592-9ece-4563-9705-1581b6e7071c}", "/Users/au443759/source/into-cps-association/maestro/maestro/src/test/resources/singlewatertank-20sim.fmu");
  		if( tankfmu == null )
  		{
  				globalExecutionContinue = false;
  				logger.log(4, "FMU load failed on fmu: '%s' for uri: '%s'", "tankFmu", "file:///Users/au443759/source/into-cps-association/maestro/maestro/src/test/resources/singlewatertank-20sim.fmu");
  				unload(logger);
  				logger = null;
  				unload(controllerfmu);
  				controllerfmu = null;
  				unload(tankfmu);
  				tankfmu = null;
  				break;
  		}
  		FMI2Component controller = controllerfmu.instantiate("controller", true, true);
  		bool controllerCurrentTimeFullStep = true;
  		real controllerCurrentTime = 0.0;
  		bool controllerBoolShare[1];
  		real controllerRealIo[4] = { 0.0 , 0.0 , 0.0 , 0.0 };
  		bool controllerBoolIo[4] = { false , false , false , false };
  		uint controllerUintVref[4] = { 0 , 0 , 0 , 0 };
  		if( controller == null )
  		{
  				globalExecutionContinue = false;
  				logger.log(4, "Instantiate failed on fmu: '%s' for instance: '%s'", "controllerFmu", "controller");
  				unload(logger);
  				logger = null;
  				unload(controllerfmu);
  				controllerfmu = null;
  				unload(tankfmu);
  				tankfmu = null;
  				break;
  		}
  		FMI2Component tank = tankfmu.instantiate("tank", true, true);
  		bool tankCurrentTimeFullStep = true;
  		real tankCurrentTime = 0.0;
  		real tankRealShare[1];
  		real tankRealIo[22] = { 0.0 , 0.0 , 0.0 , 0.0 , 0.0 , 0.0 , 0.0 , 0.0 , 0.0 , 0.0 , 0.0 , 0.0 , 0.0 , 0.0 , 0.0 , 0.0 , 0.0 , 0.0 , 0.0 , 0.0 , 0.0 , 0.0 };
  		uint tankUintVref[22] = { 0 , 0 , 0 , 0 , 0 , 0 , 0 , 0 , 0 , 0 , 0 , 0 , 0 , 0 , 0 , 0 , 0 , 0 , 0 , 0 , 0 , 0 };
  		if( tank == null )
  		{
  				globalExecutionContinue = false;
  				logger.log(4, "Instantiate failed on fmu: '%s' for instance: '%s'", "tankFmu", "tank");
  				unload(logger);
  				logger = null;
  				unload(controllerfmu);
  				controllerfmu = null;
  				unload(tankfmu);
  				tankfmu = null;
  				break;
  		}
  		tankUintVref[0] = 17;
  		tankRealIo[0] = 0.0;
  		status = tank.setReal(tankUintVref, 1, tankRealIo);
  		if( status == fmi_status_error || status == fmi_status_fatal )
  		{
  				globalExecutionContinue = false;
  				if( status == fmi_status_error )
  				{
  					logger.log(4, "SetReal failed on '%s' with status: FMI_ERROR", "tank");
  				}
  				if( status == fmi_status_fatal )
  				{
  					logger.log(4, "SetReal failed on '%s' with status: FMI_FATAL", "tank");
  				}
  				unload(logger);
  				logger = null;
  				unload(controllerfmu);
  				controllerfmu = null;
  				unload(tankfmu);
  				tankfmu = null;
0  				break;
1  		}
  		controllerUintVref[0] = 4;
  		controllerBoolIo[0] = false;
  		status = controller.setBoolean(controllerUintVref, 1, controllerBoolIo);
  		if( status == fmi_status_error || status == fmi_status_fatal )
  		{
  				globalExecutionContinue = false;
  				if( status == fmi_status_error )
  				{
  					logger.log(4, "SetBoolean failed on '%s' with status: FMI_ERROR", "controller");
  				}
  				if( status == fmi_status_fatal )
  				{
  					logger.log(4, "SetBoolean failed on '%s' with status: FMI_FATAL", "controller");
  				}
  				unload(logger);
  				logger = null;
  				unload(controllerfmu);
  				controllerfmu = null;
  				unload(tankfmu);
  				tankfmu = null;
  				break;
  		}
  		int tmp = menv.getInt("lowerlevel");
  		controllerUintVref[0] = 1;
  		controllerRealIo[0] = tmp;
  		status = controller.setReal(controllerUintVref, 1, controllerRealIo);
  		if( status == fmi_status_error || status == fmi_status_fatal )
  		{
  				globalExecutionContinue = false;
  				if( status == fmi_status_error )
  				{
  					logger.log(4, "SetReal failed on '%s' with status: FMI_ERROR", "controller");
  				}
  				if( status == fmi_status_fatal )
  				{
  					logger.log(4, "SetReal failed on '%s' with status: FMI_FATAL", "controller");
  				}
  				unload(menv);
  				menv = null;
  				unload(logger);
  				logger = null;
  				unload(controllerfmu);
  				controllerfmu = null;
  				unload(tankfmu);
  				tankfmu = null;
  				break;
  		}
  		real tmp1 = menv.getReal("upperlevel");
  		controllerUintVref[0] = 0;
  		controllerRealIo[0] = tmp1;
  		status = controller.setReal(controllerUintVref, 1, controllerRealIo);
  		if( status == fmi_status_error || status == fmi_status_fatal )
  		{
  				globalExecutionContinue = false;
  				if( status == fmi_status_error )
  				{
  					logger.log(4, "SetReal failed on '%s' with status: FMI_ERROR", "controller");
  				}
  				if( status == fmi_status_fatal )
  				{
  					logger.log(4, "SetReal failed on '%s' with status: FMI_FATAL", "controller");
  				}
  				unload(menv);
  				menv = null;
  				unload(logger);
  				logger = null;
  				unload(controllerfmu);
  				controllerfmu = null;
  				unload(tankfmu);
  				tankfmu = null;
  				break;
  		}
  		status = tank.setupExperiment(false, 0.0, 0.0, true, 11.0);
  		if( status == fmi_status_error || status == fmi_status_fatal )
  		{
  				globalExecutionContinue = false;
  				if( status == fmi_status_error )
  				{
  					logger.log(4, "SetupExperiment failed on '%s' with status: FMI_ERROR", "tank");
  				}
  				if( status == fmi_status_fatal )
  				{
  					logger.log(4, "SetupExperiment failed on '%s' with status: FMI_FATAL", "tank");
  				}
  				unload(menv);
  				menv = null;
  				unload(logger);
  				logger = null;
  				unload(controllerfmu);
  				controllerfmu = null;
  				unload(tankfmu);
  				tankfmu = null;
  				break;
  		}
  		status = controller.setupExperiment(false, 0.0, 0.0, true, 11.0);
  		if( status == fmi_status_error || status == fmi_status_fatal )
  		{
  				globalExecutionContinue = false;
  				if( status == fmi_status_error )
  				{
  					logger.log(4, "SetupExperiment failed on '%s' with status: FMI_ERROR", "controller");
  				}
  				if( status == fmi_status_fatal )
  				{
  					logger.log(4, "SetupExperiment failed on '%s' with status: FMI_FATAL", "controller");
  				}
  				unload(menv);
  				menv = null;
  				unload(logger);
  				logger = null;
  				unload(controllerfmu);
  				controllerfmu = null;
  				unload(tankfmu);
  				tankfmu = null;
  				break;
  		}
  		status = tank.enterInitializationMode();
  		if( status == fmi_status_error || status == fmi_status_fatal )
  		{
  				globalExecutionContinue = false;
  				if( status == fmi_status_error )
  				{
  					logger.log(4, "EnterInitializationMode failed on '%s' with status: FMI_ERROR", "tank");
  				}
  				if( status == fmi_status_fatal )
  				{
  					logger.log(4, "EnterInitializationMode failed on '%s' with status: FMI_FATAL", "tank");
  				}
  				unload(menv);
  				menv = null;
  				unload(logger);
  				logger = null;
  				unload(controllerfmu);
  				controllerfmu = null;
  				unload(tankfmu);
  				tankfmu = null;
  				break;
  		}
  		status = controller.enterInitializationMode();
  		if( status == fmi_status_error || status == fmi_status_fatal )
  		{
  				globalExecutionContinue = false;
  				if( status == fmi_status_error )
  				{
  					logger.log(4, "EnterInitializationMode failed on '%s' with status: FMI_ERROR", "controller");
  				}
  				if( status == fmi_status_fatal )
  				{
  					logger.log(4, "EnterInitializationMode failed on '%s' with status: FMI_FATAL", "controller");
  				}
  				unload(menv);
  				menv = null;
  				unload(logger);
  				logger = null;
  				unload(controllerfmu);
  				controllerfmu = null;
  				unload(tankfmu);
  				tankfmu = null;
  				break;
  		}
  		tankUintVref[0] = 17;
  		status = tank.getReal(tankUintVref, 1, tankRealIo);
  		if( status == fmi_status_error || status == fmi_status_fatal )
  		{
  				globalExecutionContinue = false;
  				if( status == fmi_status_error )
  				{
  					logger.log(4, "GetReal failed on '%s' with status: FMI_ERROR", "tank");
  				}
  				if( status == fmi_status_fatal )
  				{
  					logger.log(4, "GetReal failed on '%s' with status: FMI_FATAL", "tank");
  				}
  				unload(menv);
  				menv = null;
  				unload(logger);
  				logger = null;
  				unload(controllerfmu);
  				controllerfmu = null;
  				unload(tankfmu);
  				tankfmu = null;
  				break;
  		}
  		tankRealShare[0] = tankRealIo[0];
  		controllerUintVref[0] = 4;
  		status = controller.getBoolean(controllerUintVref, 1, controllerBoolIo);
  		if( status == fmi_status_error || status == fmi_status_fatal )
  		{
  				globalExecutionContinue = false;
  				if( status == fmi_status_error )
  				{
  					logger.log(4, "GetBoolean failed on '%s' with status: FMI_ERROR", "controller");
  				}
  				if( status == fmi_status_fatal )
  				{
  					logger.log(4, "GetBoolean failed on '%s' with status: FMI_FATAL", "controller");
  				}
  				unload(menv);
  				menv = null;
  				unload(logger);
  				logger = null;
  				unload(controllerfmu);
  				controllerfmu = null;
  				unload(tankfmu);
  				tankfmu = null;
  				break;
  		}
  		controllerBoolShare[0] = controllerBoolIo[0];
  		status = tank.exitInitializationMode();
  		if( status == fmi_status_error || status == fmi_status_fatal )
  		{
  				globalExecutionContinue = false;
  				if( status == fmi_status_error )
  				{
  					logger.log(4, "ExitInitializationMode failed on '%s' with status: FMI_ERROR", "tank");
  				}
  				if( status == fmi_status_fatal )
  				{
  					logger.log(4, "ExitInitializationMode failed on '%s' with status: FMI_FATAL", "tank");
  				}
  				unload(menv);
  				menv = null;
  				unload(logger);
  				logger = null;
  				unload(controllerfmu);
  				controllerfmu = null;
  				unload(tankfmu);
  				tankfmu = null;
  				break;
  		}
  		status = controller.exitInitializationMode();
  		if( status == fmi_status_error || status == fmi_status_fatal )
  		{
  				globalExecutionContinue = false;
  				if( status == fmi_status_error )
  				{
  					logger.log(4, "ExitInitializationMode failed on '%s' with status: FMI_ERROR", "controller");
  				}
  				if( status == fmi_status_fatal )
  				{
  					logger.log(4, "ExitInitializationMode failed on '%s' with status: FMI_FATAL", "controller");
  				}
  				unload(menv);
  				menv = null;
  				unload(logger);
  				logger = null;
  				unload(controllerfmu);
  				controllerfmu = null;
  				unload(tankfmu);
  				tankfmu = null;
  				break;
  		}
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
  			if( status == fmi_status_error || status == fmi_status_fatal )
  			{
  					globalExecutionContinue = false;
  					if( status == fmi_status_error )
  					{
  						logger.log(4, "SetReal failed on '%s' with status: FMI_ERROR", "tank");
  					}
  					if( status == fmi_status_fatal )
  					{
  						logger.log(4, "SetReal failed on '%s' with status: FMI_FATAL", "tank");
  					}
  					unload(menv);
  					menv = null;
  					unload(logger);
  					logger = null;
  					unload(controllerfmu);
  					controllerfmu = null;
  					unload(tankfmu);
  					tankfmu = null;
  					break;
  			}
  			controllerUintVref[0] = 3;
  			controllerRealIo[0] = tankRealShare[0];
  			status = controller.setReal(controllerUintVref, 1, controllerRealIo);
  			if( status == fmi_status_error || status == fmi_status_fatal )
  			{
  					globalExecutionContinue = false;
  					if( status == fmi_status_error )
  					{
  						logger.log(4, "SetReal failed on '%s' with status: FMI_ERROR", "controller");
  					}
  					if( status == fmi_status_fatal )
  					{
  						logger.log(4, "SetReal failed on '%s' with status: FMI_FATAL", "controller");
  					}
  					unload(menv);
  					menv = null;
  					unload(logger);
  					logger = null;
  					unload(controllerfmu);
  					controllerfmu = null;
  					unload(tankfmu);
  					tankfmu = null;
  					break;
  			}
  			status = tank.doStep(time, step_size, false);
  			if( status == fmi_status_error || status == fmi_status_fatal )
  			{
  					globalExecutionContinue = false;
  					if( status == fmi_status_error )
  					{
  						logger.log(4, "DoStep failed on '%s' with status: FMI_ERROR", "tank");
  					}
  					if( status == fmi_status_fatal )
  					{
  						logger.log(4, "DoStep failed on '%s' with status: FMI_FATAL", "tank");
  					}
  					unload(menv);
  					menv = null;
  					unload(logger);
  					logger = null;
  					unload(controllerfmu);
  					controllerfmu = null;
  					unload(tankfmu);
  					tankfmu = null;
  					break;
  			}
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
  			if( status == fmi_status_error || status == fmi_status_fatal )
  			{
  					globalExecutionContinue = false;
  					if( status == fmi_status_error )
  					{
  						logger.log(4, "DoStep failed on '%s' with status: FMI_ERROR", "controller");
  					}
  					if( status == fmi_status_fatal )
  					{
  						logger.log(4, "DoStep failed on '%s' with status: FMI_FATAL", "controller");
  					}
  					unload(menv);
  					menv = null;
  					unload(logger);
  					logger = null;
  					unload(controllerfmu);
  					controllerfmu = null;
  					unload(tankfmu);
  					tankfmu = null;
  					break;
  			}
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
  			if( status == fmi_status_error || status == fmi_status_fatal )
  			{
  					globalExecutionContinue = false;
  					if( status == fmi_status_error )
  					{
  						logger.log(4, "GetReal failed on '%s' with status: FMI_ERROR", "tank");
  					}
  					if( status == fmi_status_fatal )
  					{
  						logger.log(4, "GetReal failed on '%s' with status: FMI_FATAL", "tank");
  					}
  					unload(menv);
  					menv = null;
  					unload(logger);
  					logger = null;
  					unload(controllerfmu);
  					controllerfmu = null;
  					unload(tankfmu);
  					tankfmu = null;
  					break;
  			}
  			tankRealShare[0] = tankRealIo[0];
  			controllerUintVref[0] = 4;
  			status = controller.getBoolean(controllerUintVref, 1, controllerBoolIo);
  			if( status == fmi_status_error || status == fmi_status_fatal )
  			{
  					globalExecutionContinue = false;
  					if( status == fmi_status_error )
  					{
  						logger.log(4, "GetBoolean failed on '%s' with status: FMI_ERROR", "controller");
  					}
  					if( status == fmi_status_fatal )
  					{
  						logger.log(4, "GetBoolean failed on '%s' with status: FMI_FATAL", "controller");
  					}
  					unload(menv);
  					menv = null;
  					unload(logger);
  					logger = null;
  					unload(controllerfmu);
  					controllerfmu = null;
  					unload(tankfmu);
  					tankfmu = null;
  					break;
  			}
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
  		if( logger != null )
  		{
  				unload(logger);
  		}
  		if( menv != null )
  		{
  				unload(menv);
  		}
  		break;
  	}
  }

```

