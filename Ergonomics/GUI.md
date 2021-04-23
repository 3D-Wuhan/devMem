# GUI


## 1. Main QActions


### 1.1 Definition of the QActions
ln546 in mainwindow.h
```cpp
	QAction	*projAct;	// added by jicheng, original projection action, not used any more
	QAction *trainAct;	// added by jicheng, select files from a folder to train
	QAction	*synthAct;	// added by jicheng, synthesis of all the trained results
	QAction	*judgeAct;	// added by jicheng, evaluation the designation
```

the corresponding slots are in ln251 of mainwindow.h
```cpp
	bool	batchTrain ( QString fileName = QString( ) );	// QAction	*trainAct
	bool	makeSynthesis ();				// QAction	*synthAct
	bool	evaluatingJudge ();				// QAction	*judgeAct
```

add to toolbar, ln542 in mainwindow_init.cpp
```cpp
	mainToolBar->addAction ( projAct );
	mainToolBar->addAction ( trainAct );
	mainToolBar->addAction ( synthAct );
	mainToolBar->addAction ( judgeAct );
```

## 2. Adding vertices to a model

Please refer to [allocating and deleting](../meshLab/vcglib/Allocating_n_Deleting.md).

