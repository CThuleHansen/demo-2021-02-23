# cpp codegen
```cpp
#include "co-sim.hxx"
#import <stdint.h>
#import <string>
#import "SimFmi2.h"
#import "MEnv.h"
void simulate()
{
	int globalExecutionContinue = true;
	int fmi_status_discard = 2;
	int fmi_status_ok = 0;
	int status;
	while(globalExecutionContinue)
	{
		MEnv menv = load_MEnv();
		FMI2 controllerfmu = load_FMI2("{8c4e810f-3df3-4a00-8276-176fa3c9f000}", "/Users/au443759/source/into-cps-association/maestro/maestro/src/test/resources/watertankcontroller-c.fmu");
		FMI2 tankfmu = load_FMI2("{cfc65592-9ece-4563-9705-1581b6e7071c}", "/Users/au443759/source/into-cps-association/maestro/maestro/src/test/resources/singlewatertank-20sim.fmu");
		FMI2Component controller = controllerfmu->instantiate("controller", true, true);
		int controllerCurrentTimeFullStep = true;
		double controllerCurrentTime = 0.0;
		int controllerBoolShare[1];
		double controllerRealIo[4] = {0.0, 0.0, 0.0, 0.0, };
		int controllerBoolIo[4] = {false, false, false, false, };
		unsigned int controllerUintVref[4] = {0, 0, 0, 0, };
		FMI2Component tank = tankfmu->instantiate("tank", true, true);
		int tankCurrentTimeFullStep = true;
		double tankCurrentTime = 0.0;
		double tankRealShare[1];
		double tankRealIo[22] = {0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, };
		unsigned int tankUintVref[22] = {0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, };
		tankUintVref[0] = 17;
		tankRealIo[0] = 0.0;
		status = tank->fmu->setReal(tank->comp, tankUintVref, 1, tankRealIo);
		controllerUintVref[0] = 4;
		controllerBoolIo[0] = false;
		status = controller->fmu->setBoolean(controller->comp, controllerUintVref, 1, controllerBoolIo);
		int tmp = menv->getInt("lowerlevel");
		controllerUintVref[0] = 1;
		controllerRealIo[0] = tmp;
		status = controller->fmu->setReal(controller->comp, controllerUintVref, 1, controllerRealIo);
		double tmp1 = menv->getReal("upperlevel");
		controllerUintVref[0] = 0;
		controllerRealIo[0] = tmp1;
		status = controller->fmu->setReal(controller->comp, controllerUintVref, 1, controllerRealIo);
		status = tank->fmu->setupExperiment(tank->comp, false, 0.0, 0.0, true, 11.0);
		status = controller->fmu->setupExperiment(controller->comp, false, 0.0, 0.0, true, 11.0);
		status = tank->fmu->enterInitializationMode(tank->comp);
		status = controller->fmu->enterInitializationMode(controller->comp);
		tankUintVref[0] = 17;
		status = tank->fmu->getReal(tank->comp, tankUintVref, 1, tankRealIo);
		tankRealShare[0] = tankRealIo[0];
		controllerUintVref[0] = 4;
		status = controller->fmu->getBoolean(controller->comp, controllerUintVref, 1, controllerBoolIo);
		controllerBoolShare[0] = controllerBoolIo[0];
		status = tank->fmu->exitInitializationMode(tank->comp);
		status = controller->fmu->exitInitializationMode(controller->comp);
		double time = 0.0;
		double step_size = 0.1;
		while(time + step_size < 10.0)
		{
			tankUintVref[0] = 16;
			if(controllerBoolShare[0])
			{
				tankRealIo[0] = 1.0;
			}
			else
			{
				tankRealIo[0] = 0.0;
			}

			status = tank->fmu->setReal(tank->comp, tankUintVref, 1, tankRealIo);
			controllerUintVref[0] = 3;
			controllerRealIo[0] = tankRealShare[0];
			status = controller->fmu->setReal(controller->comp, controllerUintVref, 1, controllerRealIo);
			status = tank->fmu->doStep(tank->comp, time, step_size, false);
			if(status != fmi_status_ok)
			{
				if(status == fmi_status_discard)
				{
					status = tank->fmu->getRealStatus(tank->comp, fmi2LastSuccessfulTime, &tankCurrentTime);
					tankCurrentTimeFullStep = false;
				}

			}
			else
			{
				tankCurrentTime = time + step_size;
				tankCurrentTimeFullStep = true;
			}

			status = controller->fmu->doStep(controller->comp, time, step_size, false);
			if(status != fmi_status_ok)
			{
				if(status == fmi_status_discard)
				{
					status = controller->fmu->getRealStatus(controller->comp, fmi2LastSuccessfulTime, &controllerCurrentTime);
					controllerCurrentTimeFullStep = false;
				}

			}
			else
			{
				controllerCurrentTime = time + step_size;
				controllerCurrentTimeFullStep = true;
			}

			tankUintVref[0] = 17;
			status = tank->fmu->getReal(tank->comp, tankUintVref, 1, tankRealIo);
			tankRealShare[0] = tankRealIo[0];
			controllerUintVref[0] = 4;
			status = controller->fmu->getBoolean(controller->comp, controllerUintVref, 1, controllerBoolIo);
			controllerBoolShare[0] = controllerBoolIo[0];
			time = time + step_size;
		}

		if(controllerfmu != nullptr)
		{
			;
			controllerfmu = nullptr;
		}

		if(tankfmu != nullptr)
		{
			;
			tankfmu = nullptr;
		}

		if(menv != nullptr)
		{
			;
		}

		break;
	}

}
```
