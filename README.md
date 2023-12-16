turbopuffer Python Client
=========================

The official Python client for accessing the turbopuffer API.

Usage
-----

1. Install the turbopuffer package and set your API key.
```sh
$ pip install turbopuffer
```
Or if you're able to run C binaries for JSON encoding, use:
```sh
$ pip install turbopuffer[fast]
```

2. Start using the API
```py
import turbopuffer as tpuf
tpuf.api_key = 'your-token' # Alternatively: export=TURBOPUFFER_API_KEY=your-token

# Open a namespace
ns = tpuf.Namespace('hello_world')

# Upsert your dataset
ns.upsert(
    ids=[1, 2],
    vectors=[[0.1, 0.2], [0.3, 0.4]],
    attributes={'name': ['foo', 'foos']}
)

# Alternatively, upsert using a row iterator
ns.upsert(
    {
        'id': id,
        'vector': [id/10, id/10],
        'attributes': {'name': 'food'}
    } for id in range(3, 10)
)

# Query your dataset
vectors = ns.query(
    vector=[0.15, 0.22],
    distance_metric='cosine_distance',
    top_k=10,
    filters={ 'name': [['Glob', 'foo*'], ['NotEq', 'food']] },
    include_attributes=['name'],
    include_vectors=True
)
print(vectors)
# [
#   VectorRow(id=2, vector=[0.30000001192092896, 0.4000000059604645], attributes={'name': 'foos'}, dist=0.001016080379486084),
#   VectorRow(id=1, vector=[0.10000000149011612, 0.20000000298023224], attributes={'name': 'foo'}, dist=0.009067952632904053)
# ]
```
