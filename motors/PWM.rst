===
PWM
===

Creating a New PWM Motor Controller
-----------------------------------

All motor controllers follow the form ``Type name = new Type(channel)`` where ``channel`` is the channel of the roboRIO the port is plugged in.

.. tabs::

    .. code-tab:: java

        Talon shooter = new Talon(2)
        VictorSP intake = new VictorSP(3)


    .. code-tab:: c++

        Talon *shooter = new Talon(2);
        VictorSP *intake = new VictorSP(3);

    .. code-tab:: py

        shooter = wpilib.Talon(2)
        intake = wpilib.VictorSP(3)


Sending Output to Motors
------------------------
Both PWM and CAN motor controllers use the ``set`` method to control the motors.

.. tabs::

    .. code-tab:: java

        shooter.set(1);
        intake.set(.5);


    .. code-tab:: c++

        shooter->set(1);
        intake->set(.5);

    .. code-tab:: py

        self.shooter.set(1)
        self.intake.set(.5)
