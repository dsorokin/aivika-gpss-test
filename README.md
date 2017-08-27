
# A Test Simulation Model Based on Using GPSS-like DSL of Aivika

This test project demonstrates how to use the GPSS-like DSL of package [aivika-gpss](http://hackage.haskell.org/package/aivika-gpss) from the [Aivika](http://www.aivikasoft.com) simulation framework.

## GPSS Model

The demo is based on the following GPSS model:

```
        GENERATE 2000,500,,,1
        GATE NI PROF,Busy
        PREEMPT PROF,PR,Add,5
        ADVANCE (Exponential(1,0,200))
        RETURN PROF
Busy    TERMINATE

        GENERATE 2000,500
        QUEUE LINE
        SEIZE PROF
        DEPART LINE
        ADVANCE (Exponential(1,0,1000))
LetGo   RELEASE PROF
        TERMINATE

Add     ASSIGN 5+,300
        ADVANCE P5
        TRANSFER ,LetGo

        GENERATE 10000000
        TERMINATE 1

        START 1
```

## Equivalent Aivika Model

An equivalent code written in Haskell and based on using the GPSS-like DSL of Aivika is located in the [Main.hs](https://github.com/dsorokin/aivika-gpss-test/blob/master/app/Main.hs) file.

### Prerequisites

Since the code is written in Haskell, in the simplest case you need [Stack](http://docs.haskellstack.org/) installed on your computer. But to reproduce the test, you don't need to know the Haskell programming language, though.

### Downloading from GitHub

Download the test code from GitHub:

```
$ git clone https://github.com/dsorokin/aivika-gpss-test.git
$ cd aivika-gpss-test
```

### Building Project

For the first time, you will have to set up the Stack project. In the next time, you can skip this step.

`$ stack setup`

In the beginning and after each change of the corresponding Haskell code, you have to build a binary executable anew.

`$ stack build`

### Running Simulation

To run the simulation, type in the Terminal window:

`$ stack exec aivika-gpss-test`

### Simulation Results

The results will differ from time to time, although they are quite stable statistically. When reading them, please note that some properties have slightly other names, but their meaning is very close to GPSS.

For example, in my case I received the following output:

```
----------

-- simulation time
t = 1.0e7

-- Line
line:

  -- is the queue empty?
  queueNull = False

  -- the current queue content
  queueContent = 2

  -- the queue content statistics
  queueContentStats = { count = 9979, mean = 0.5210154651068638, std = 1.04087571549751, min = 0 (t = 0.0), max = 11 (t = 8593437.920672985), t in [0.0, 9999142.117547587] }

  -- the number of enqueued transacts
  enqueueCount = 4990

  -- the number of zero entry enqueued transacts
  enqueueZeroEntryCount = 2552

  -- the wait time
  queueWaitTime = { count = 4988, mean = 1043.974895169938, std = 2013.029167489926, min = 0.0, max = 20431.66174436547 }

  -- the wait time without zero entries
  queueNonZeroEntryWaitTime = { count = 2436, mean = 2137.6628805860746, std = 2441.390169860242, min = 0.6721845287829638, max = 20431.66174436547 }

  -- the average queue rate (= queue size / wait time)
  queueRate = 4.990689599121567e-4

-- Prof
prof:

  -- the current queue size
  queueCount = 2

  -- the queue size statistics
  queueCountStats = { count = 10969, mean = 0.5810955624486054, std = 1.0838206657395248, min = 0 (t = 0.0), max = 12 (t = 8593517.679018965), t in [0.0, 9999668.9882097] }

  -- the total wait time
  totalWaitTime = 5807348.629743358

  -- the wait time
  waitTime = { count = 13030, mean = 445.6906085758505, std = 1337.1000085478483, min = 0.0, max = 20431.66174436547 }

  -- the total holding time
  totalHoldingTime = 6963464.912808844

  -- the holding time
  holdingTime = { count = 9982, mean = 697.602175196236, std = 1004.5336697749345, min = 1.7071446403861046e-2, max = 10003.1510077063 }

  -- is the facility interrupted now?
  interrupted = False

  -- the current available count
  count = 0

  -- the available count statistics
  countStats = { count = 9000, mean = 0.30391905402166214, std = 0.45994810862122276, min = 0 (t = 1983.2761628497485), max = 1 (t = 0.0), t in [0.0, 9988565.300713118] }

  -- the current capture count
  captureCount = 9983

  -- the current utilisation count
  utilisationCount = 1

  -- the utilisation count statistics
  utilisationCountStats = { count = 19966, mean = 0.6964184193691163, std = 0.45980409364482383, min = 0 (t = 0.0), max = 1 (t = 1983.2761628497485), t in [0.0, 9999668.9882097] }
```
