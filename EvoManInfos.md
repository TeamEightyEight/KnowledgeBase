# EvoMan KB

## Classes to take care of

### `Environment`

Sets the game settings up. Needed to run experiments, comparable to a OpenAI Gym `Environment`. Initialized with

```python
# initializes simulation in individual evolution mode, for single static enemy.
env = Environment(experiment_name=experiment_name,
                  enemies=[2],
                  playermode="ai",
                  player_controller=player_controller(n_hidden_neurons),
                  enemymode="static",
                  level=2,
                  speed="fastest")
```

To check `Environment`'s parameters, see [docs at page 9](https://github.com/karinemiras/evoman_framework/blob/master/evoman1.0-doc.pdf).

To run experiments, we use `env.play(pcont, econt)` providing two `Controller`s. This function returns `fitness`, `player_life`, `enemy_life`, `game_runtime` as lists.

With `fitness_single()` we can get the solution's fitness. This should be intersting to customize.

If we have multiple fitnesses, the `cons_multi(vals)` method normalizes them into one single value.

### `Controller`

The actual thing we'll have to work on. To implement our **controller**, we'll have to extend the `Controller` class:

```python
class player_controller(Controller):
	def __init__(self):
		pass
 	def control(self, inputs, controller):
		pass
```

The `control` method is the core of it all: taking the sensor data as input, it returns a `List` of booleans containing the actions to take.

`inputs` is a numpy array, while the return list is composed as follows:

```python
[left, right, jump, shoot, release]
```

The `controller` parameter is provided by `environment.play()` and could either contain weight, or a full network strucutre (e.g. from NEAT).

Note that by default, `Controller` acts randomly, as this is the library's code:

```python
class Controller(object):
    def control(self, params, cont = None):
        action1 = numpy.random.choice([1,0])
        action2 = numpy.random.choice([1,0])
        action3 = numpy.random.choice([1,0])
        action4 = numpy.random.choice([1,0])
        action5 = numpy.random.choice([1,0])
        action6 = numpy.random.choice([1,0])

        return [action1, action2, action3, action4, action5, action6]
```
