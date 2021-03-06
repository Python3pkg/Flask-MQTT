Usage
=====

Connect to a broker
-------------------

To connect to a broker you only need to initialize the `Flask-MQTT` extension
with your `Flask application`. You can do this by directly passing the `Flask
application object`_ on object creation.

::

    from flask import Flask
    from flask_mqtt import Mqtt

    app = Flask(__name__)
    mqtt = Mqtt(app)

The *Flask-MQTT* extension supports the factory pattern so you can instantiate
a ``Mqtt`` object without an app object. Use the ``init_app()`` function inside
the factory function for initialization.

::

    from flask import Flask
    from flask_mqtt import Mqtt

    mqtt = Mqtt()

    def create_app():
        app = Flask(__name__)
        mqtt.init_app(app)

Subscribe to a topic
--------------------
To subscribe to a topic simply use :py:func:`flask_mqtt.Mqtt.subscribe`.

::

    mqtt.subscribe('home/mytopic')

To handle the subscribed messages you can define a handling function by
using the :py:func:`flask_mqtt.Mqtt.on_message` decorator.

::

    @mqtt.on_message()
    def handle_mqtt_message(client, userdata, message):
        data = dict(
            topic=message.topic,
            payload=message.payload.decode()
        )

To unsubscribe use :py:func:`flask_mqtt.Mqtt.unsubscribe`.

::

    mqtt.unsubscribe('home/mytopic')

Or if you want to unsubscribe all topics use
:py:func:`flask_mqtt.Mqtt.unsubscribe_all`.

::

    mqtt.unsubscribe_all()


Publish a message
-----------------
Publishing a message is easy. Just use the :py:func:`flask_mqtt.Mqtt.publish`
method here.

::

    mqtt.publish('home/mytopic', 'hello world')

Interact with SocketIO
----------------------

.. _Flask application object: http://flask.pocoo.org/docs/0.12/api/#application-object