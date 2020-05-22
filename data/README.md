## Dataset

We use public [Amazon dataset](http://jmcauley.ucsd.edu/data/amazon/links.html) (*Grocery_and_Gourmet_Food* category, 5-core version with metadata) as our build-in dataset. You can modify the `DATASET` variable in `preprocess.ipynb` to download and build datasets for other categories.



Our framework can also easily work with other datasets. We describe the required files below (recommend to open `preprocess.ipynb` to observe the format of dataset files):



**train.csv**

- Format: `user_id \t item_id \t time`
- All ids begin from 1 (0 is reserved for NaN), and the followings are the same.
- Need to be sorted in time-ascending order when running sequential models.



**test.csv & dev.csv**

- Format: `user_id \t item_id \t time \t neg_items`
- The last column is the list of negative items in terms of the ground-truth item.
- The number of negative items need to be the same for a specific set, but it can be different between dev and test sets.

![dev/test data format](http://snappyimages.nextwavesrl.netdna-cdn.com/img/d5427cd138ad9ec87190973cb5ffcfc9.png)



**item_meta.csv** (opt)

- Format: `item_id \t <attribute> \t ... \t r_<relation> \t ...`
- Optional, only needed for some of the models (CFKG, SLRC, Chorus).
- `<attribute>` is the attributes of an item, such as category, brand and so on. SLRC and Chorus model need category information.
- `r_<relation>` is the relations between items, and its value is a list of items (can be empty []). Assume `item_id` is `i`, if `j` appears in `r_<relation>`, then `(i, relation, j)` holds in the knowledge graph. Note that the corresponding header here must start with "r_" to be distinguished from attributes.

![dev/test data format](http://snappyimages.nextwavesrl.netdna-cdn.com/img/56e1473d62039d4853e8360731640930.png)



You can also implement a new loader class based on [BaseLoader.py](https://github.com/THUwangcy/ReChorus/tree/master/src/helpers/BaseLoader.py) and load data in your own style, as long as the basic information is included. Then assign your model with the new loader and begin to use new members of the loader when preparing batches in the model.
