## 一、介绍

TensorFlow Datasets是一个开箱即用的数据集集合，包含数十种常用的机器学习数据集。
TensorFlow Datasets不包含在tensorflow中，而是一个独立的包，使用的话需要安装：

```bash
pip install tensorflow-datasets
```



## 二、使用

```python
import tensorflow as tf
import tensorflow_datasets as tfds

dataset1=tfds.load('mnist',split=tfds.Split.TRAIN) # 手写数字识别数据集
dataset2=tfds.load("cats_vs_dogs",split=tfds.Split.TRAIN,as_supervised=True) # 猫狗识别
dataset3=tfds.load("tf_flowers",split=tfds.Split.TRAIN,as_supervised=True) # tf_flowers图像分类数据集

```

* tfdf.load返回一个tf.data.Dataset对象
* split：指定返回数据集的特定部分，不然返回所有。tfds.Split.TRAIN（训练集），tfds.Split.TEST（测试集）
* as_supervised：若为True，则根据数据集的特性，将数据集中的每行元素整理为由监督的二元组（input,label），即（数据+标签），否则数据集每行元素为包含所有特征的字典。
* tfds.list_builders()可查看支持的数据集



支持数据集

```python
['abstract_reasoning',
 'accentdb',
 'aeslc',
 'aflw2k3d',
 'ag_news_subset',
 'ai2_arc',
 'ai2_arc_with_ir',
 'amazon_us_reviews',
 'anli',
 'answer_equivalence',
 'arc',
 'asqa',
 'asset',
 'assin2',
 'bair_robot_pushing_small',
 'bccd',
 'beans',
 'bee_dataset',
 'beir',
 'big_patent',
 'bigearthnet',
 'billsum',
 'binarized_mnist',
 'binary_alpha_digits',
 'ble_wind_field',
 'blimp',
 'booksum',
 'bool_q',
 'c4',
 'caltech101',
 'caltech_birds2010',
 'caltech_birds2011',
 'cardiotox',
 'cars196',
 'cassava',
 'cats_vs_dogs',
 'celeb_a',
 'celeb_a_hq',
 'cfq',
 'cherry_blossoms',
 'chexpert',
 'cifar10',
 'cifar100',
 'cifar10_1',
 'cifar10_corrupted',
 'citrus_leaves',
 'cityscapes',
 'civil_comments',
 'clevr',
 'clic',
 'clinc_oos',
 'cmaterdb',
 'cnn_dailymail',
 'coco',
 'coco_captions',
 'coil100',
 'colorectal_histology',
 'colorectal_histology_large',
 'common_voice',
 'coqa',
 'cos_e',
 'cosmos_qa',
 'covid19',
 'covid19sum',
 'crema_d',
 'criteo',
 'cs_restaurants',
 'curated_breast_imaging_ddsm',
 'cycle_gan',
 'd4rl_adroit_door',
 'd4rl_adroit_hammer',
 'd4rl_adroit_pen',
 'd4rl_adroit_relocate',
 'd4rl_antmaze',
 'd4rl_mujoco_ant',
 'd4rl_mujoco_halfcheetah',
 'd4rl_mujoco_hopper',
 'd4rl_mujoco_walker2d',
 'dart',
 'davis',
 'deep1b',
 'deep_weeds',
 'definite_pronoun_resolution',
 'dementiabank',
 'diabetic_retinopathy_detection',
 'diamonds',
 'div2k',
 'dmlab',
 'doc_nli',
 'dolphin_number_word',
 'domainnet',
 'downsampled_imagenet',
 'drop',
 'dsprites',
 'dtd',
 'duke_ultrasound',
 'e2e_cleaned',
 'efron_morris75',
 'emnist',
 'eraser_multi_rc',
 'esnli',
 'eurosat',
 'fashion_mnist',
 'flic',
 'flores',
 'food101',
 'forest_fires',
 'fuss',
 'gap',
 'geirhos_conflict_stimuli',
 'gem',
 'genomics_ood',
 'german_credit_numeric',
 'gigaword',
 'glove100_angular',
 'glue',
 'goemotions',
 'gov_report',
 'gpt3',
 'gref',
 'groove',
 'grounded_scan',
 'gsm8k',
 'gtzan',
 'gtzan_music_speech',
 'hellaswag',
 'higgs',
 'hillstrom',
 'horses_or_humans',
 'howell',
 'i_naturalist2017',
 'i_naturalist2018',
 'imagenet2012',
 'imagenet2012_corrupted',
 'imagenet2012_fewshot',
 'imagenet2012_multilabel',
 'imagenet2012_real',
 'imagenet2012_subset',
 'imagenet_a',
 'imagenet_lt',
 'imagenet_r',
 'imagenet_resized',
 'imagenet_sketch',
 'imagenet_v2',
 'imagenette',
 'imagewang',
 'imdb_reviews',
 'irc_disentanglement',
 'iris',
 'istella',
 'kddcup99',
 'kitti',
 'kmnist',
 'lambada',
 'lfw',
 'librispeech',
 'librispeech_lm',
 'libritts',
 'ljspeech',
 'lm1b',
 'locomotion',
 'lost_and_found',
 'lsun',
 'lvis',
 'malaria',
 'math_dataset',
 'math_qa',
 'mctaco',
 'media_sum',
 'mlqa',
 'mnist',
 'mnist_corrupted',
 'movie_lens',
 'movie_rationales',
 'movielens',
 'moving_mnist',
 'mrqa',
 'mslr_web',
 'mt_opt',
 'multi_news',
 'multi_nli',
 'multi_nli_mismatch',
 'natural_questions',
 'natural_questions_open',
 'newsroom',
 'nsynth',
 'nyu_depth_v2',
 'ogbg_molpcba',
 'omniglot',
 'open_images_challenge2019_detection',
 'open_images_v4',
 'openbookqa',
 'opinion_abstracts',
 'opinosis',
 'opus',
 'oxford_flowers102',
 'oxford_iiit_pet',
 'para_crawl',
 'pass',
 'patch_camelyon',
 'paws_wiki',
 'paws_x_wiki',
 'penguins',
 'pet_finder',
 'pg19',
 'piqa',
 'places365_small',
 'plant_leaves',
 'plant_village',
 'plantae_k',
 'protein_net',
 'qa4mre',
 'qasc',
 'quac',
 'quality',
 'quickdraw_bitmap',
 'race',
 'radon',
 'reddit',
 'reddit_disentanglement',
 'reddit_tifu',
 'ref_coco',
 'resisc45',
 'rlu_atari',
 'rlu_atari_checkpoints',
 'rlu_atari_checkpoints_ordered',
 'rlu_control_suite',
 'rlu_dmlab_explore_object_rewards_few',
 'rlu_dmlab_explore_object_rewards_many',
 'rlu_dmlab_rooms_select_nonmatching_object',
 'rlu_dmlab_rooms_watermaze',
 'rlu_dmlab_seekavoid_arena01',
 'rlu_locomotion',
 'rlu_rwrl',
 'robomimic_ph',
 'robonet',
 'robosuite_panda_pick_place_can',
 'rock_paper_scissors',
 'rock_you',
 's3o4d',
 'salient_span_wikipedia',
 'samsum',
 'savee',
 'scan',
 'scene_parse150',
 'schema_guided_dialogue',
 'sci_tail',
 'scicite',
 'scientific_papers',
 'scrolls',
 'sentiment140',
 'shapes3d',
 'sift1m',
 'simpte',
 'siscore',
 'smallnorb',
 'smartwatch_gestures',
 'snli',
 'so2sat',
 'speech_commands',
 'spoken_digit',
 'squad',
 'squad_question_generation',
 'stanford_dogs',
 'stanford_online_products',
 'star_cfq',
 'starcraft_video',
 'stl10',
 'story_cloze',
 'summscreen',
 'sun397',
 'super_glue',
 'svhn_cropped',
 'symmetric_solids',
 'tao',
 'ted_hrlr_translate',
 'ted_multi_translate',
 'tedlium',
 'tf_flowers',
 'the300w_lp',
 'tiny_shakespeare',
 'titanic',
 'trec',
 'trivia_qa',
 'tydi_qa',
 'uc_merced',
 'ucf101',
 'unified_qa',
 'vctk',
 'visual_domain_decathlon',
 'voc',
 'voxceleb',
 'voxforge',
 'waymo_open_dataset',
 'web_graph',
 'web_nlg',
 'web_questions',
 'wider_face',
 'wiki40b',
 'wiki_auto',
 'wiki_bio',
 'wiki_dialog',
 'wiki_table_questions',
 'wiki_table_text',
 'wikiann',
 'wikihow',
 'wikipedia',
 'wikipedia_toxicity_subtypes',
 'wine_quality',
 'winogrande',
 'wit',
 'wit_kaggle',
 'wmt13_translate',
 'wmt14_translate',
 'wmt15_translate',
 'wmt16_translate',
 'wmt17_translate',
 'wmt18_translate',
 'wmt19_translate',
 'wmt_t2t_translate',
 'wmt_translate',
 'wordnet',
 'wsc273',
 'xnli',
 'xquad',
 'xsum',
 'xtreme_pawsx',
 'xtreme_s',
 'xtreme_xnli',
 'yelp_polarity_reviews',
 'yes_no',
 'youtube_vis']
```

