# config-transform-random
Tool for transforming AllenNLP config using random setting.

## Define random transformations
Use selected random functions from python random using named arguments.

Input:
```json
{
    "uniform_value": {"type":"random", "func":"uniform", "args":{"a": 0.0, "b": 0.2}},
    "randint_value": {"type":"random", "func":"randint", "args":{"a": 100, "b": 500}},
    "randrange_value": {"type":"random", "func":"randrange", "args":{"start":10, "stop":100, "step":10}},
    "shuffle_value": {"type":"random", "func":"shuffle", "args":{"x":[1,"2", 3, "4"]}},
    "choice_value": {"type":"random", "func":"choice", "args":{"seq":["lstm", "gru"]}},
    "choices_value": {"type":"random", "func":"choices", "args":{"population":["lstm", "gru"], "weights":[2, 2], "k":1}},
    "choices_first_value": {"type":"random", "func":"choices[0]", "args":{"population":["lstm", "gru"], "weights":[2, 2], "k":1}},
    "choices_first_value_list": [{"value":{"type":"random", "func":"choices[0]", "args":{"population":["lstm", "gru"], "weights":[2, 2], "k":1}}}],
}
```

Generated output:
```json
{
    "choice_value": "lstm",
    "choices_first_value": "gru",
    "choices_first_value_list": [
        {
            "value": "gru"
        }
    ],
    "choices_value": [
        "lstm"
    ],
    "randint_value": 130,
    "randrange_value": 20,
    "shuffle_value": "None",
    "uniform_value": 0.15845503640889036
}
```

## Usage

- Having a single random file to transform:

```bash
python config_transform.py -i test_config_random.json > out_transformed_config.json
```

- Having a static or random file where we want to apply *only* the random transformations:

```bash
# Note 1: Transformations from test_config_random.json are applied for the corresponding hierarchical property in test_config_static.json if it exists.
# Note 2: Transformations are applied for -i test_config_static.json if it has defined setting with "type":"random"

python config_transform.py -i test_config_static.json -po test_config_random.json > out_transformed_config.json
```

- Having a static or random file where we want to apply some *static* transofrmations :

```bash
python config_transform.py -i test_config_static.json -t "choice_value:test" > out_transformed_config.json
```



## Parameters
```txt
usage: config_transform.py [-h] [-i CONFIG_FILE]
                                      [-t level1->level2:5|field2:[1,2,3]]
                                      [-po PARAMS_OVERRIDES]
                                      [-o NEW_CONFIG_FILE]

Transform config file given a list of json hierachical transformations

optional arguments:
  -h, --help            show this help message and exit
  -i CONFIG_FILE, --input_file CONFIG_FILE
                        Input config file to transform. The file is loaded and
                        json settings with type random are evaluated.
  -t level1->level2:5|field2:[1,2,3], --transformations level1->level2:5|field2:[1,2,3]
                        transformations to apply
  -po PARAMS_OVERRIDES, --params_overrides PARAMS_OVERRIDES
                        Params overrides to apply. This is a json file with
                        updated values. It can also be a path to a json file!.
  -o NEW_CONFIG_FILE, --output_file NEW_CONFIG_FILE
                        File to save the new config

```