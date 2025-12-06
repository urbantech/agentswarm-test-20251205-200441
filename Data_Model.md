# Data Model - TaskMaster Pro

**Generated**: 2025-12-06T04:20:59.844516
**App Type**: task_management
**Database**: ZeroDB

## Overview

This data model uses ZeroDB, which combines:
- **SQL Tables**: For relational data (users, projects, tasks)
- **Vector Collections**: For semantic search (task similarity)
- **Memory Tables**: For caching and real-time updates

## SQL Tables

{}

## Vector Collections

{
  "entities": {
    "name": "entities",
    "description": "Primary application entities",
    "vector_fields": [
      "content_embedding",
      "metadata_embedding"
    ],
    "metadata_schema": {
      "entity_id": "string",
      "name": "string",
      "description": "text",
      "status": "string",
      "category": "string",
      "tags": "array",
      "created_at": "timestamp",
      "updated_at": "timestamp"
    },
    "vector_config": {
      "enabled": true,
      "dimension": 1536,
      "metric": "cosine",
      "fields": {
        "content_embedding": {
          "dimension": 1536,
          "metric": "cosine",
          "index_type": "hnsw",
          "index_params": {
            "ef_construct": 128,
            "m": 16
          }
        },
        "metadata_embedding": {
          "dimension": 1536,
          "metric": "cosine",
          "index_type": "hnsw",
          "index_params": {
            "ef_construct": 128,
            "m": 16
          }
        }
      }
    },
    "indexes": [
      {
        "name": "idx_vector_content_embedding",
        "type": "hnsw",
        "field": "content_embedding",
        "parameters": {
          "ef_construct": 128,
          "m": 16,
          "metric": "cosine"
        }
      },
      {
        "name": "idx_vector_metadata_embedding",
        "type": "hnsw",
        "field": "metadata_embedding",
        "parameters": {
          "ef_construct": 128,
          "m": 16,
          "metric": "cosine"
        }
      }
    ],
    "validation_rules": {
      "entity_id": [
        {
          "type": "string",
          "min_length": 1,
          "max_length": 255
        }
      ],
      "name": [
        {
          "type": "string",
          "min_length": 1,
          "max_length": 255
        }
      ],
      "description": [
        {
          "type": "string",
          "min_length": 1,
          "max_length": 10000
        }
      ],
      "status": [
        {
          "type": "string",
          "min_length": 1,
          "max_length": 255
        }
      ],
      "category": [
        {
          "type": "string",
          "min_length": 1,
          "max_length": 255
        }
      ],
      "tags": [
        {
          "type": "array",
          "max_items": 100
        }
      ],
      "created_at": [
        {
          "type": "string",
          "format": "date-time"
        }
      ],
      "updated_at": [
        {
          "type": "string",
          "format": "date-time"
        }
      ]
    },
    "access_patterns": []
  },
  "users": {
    "name": "users",
    "description": "Application users",
    "vector_fields": [
      "profile_embedding"
    ],
    "metadata_schema": {
      "user_id": "string",
      "email": "string",
      "name": "string",
      "role": "string",
      "created_at": "timestamp",
      "last_active": "timestamp"
    },
    "vector_config": {
      "enabled": true,
      "dimension": 1536,
      "metric": "cosine",
      "fields": {
        "profile_embedding": {
          "dimension": 1536,
          "metric": "cosine",
          "index_type": "hnsw",
          "index_params": {
            "ef_construct": 128,
            "m": 16
          }
        }
      }
    },
    "indexes": [
      {
        "name": "idx_vector_profile_embedding",
        "type": "hnsw",
        "field": "profile_embedding",
        "parameters": {
          "ef_construct": 128,
          "m": 16,
          "metric": "cosine"
        }
      }
    ],
    "validation_rules": {
      "user_id": [
        {
          "type": "string",
          "min_length": 1,
          "max_length": 255
        }
      ],
      "email": [
        {
          "type": "string",
          "min_length": 1,
          "max_length": 255
        },
        {
          "format": "email"
        }
      ],
      "name": [
        {
          "type": "string",
          "min_length": 1,
          "max_length": 255
        }
      ],
      "role": [
        {
          "type": "string",
          "min_length": 1,
          "max_length": 255
        }
      ],
      "created_at": [
        {
          "type": "string",
          "format": "date-time"
        }
      ],
      "last_active": [
        {
          "type": "string",
          "format": "date-time"
        }
      ]
    },
    "access_patterns": [
      {
        "name": "user_lookup",
        "query": {
          "email": "?"
        },
        "frequency": "high"
      },
      {
        "name": "active_users",
        "query": {
          "status": "active"
        },
        "frequency": "medium"
      },
      {
        "name": "recent_users",
        "sort": {
          "created_at": -1
        },
        "frequency": "low"
      }
    ]
  },
  "interactions": {
    "name": "interactions",
    "description": "User interactions and activities",
    "vector_fields": [
      "action_embedding",
      "context_embedding"
    ],
    "metadata_schema": {
      "interaction_id": "string",
      "user_id": "string",
      "entity_id": "string",
      "action": "string",
      "context": "json",
      "timestamp": "timestamp"
    },
    "vector_config": {
      "enabled": true,
      "dimension": 1536,
      "metric": "cosine",
      "fields": {
        "action_embedding": {
          "dimension": 1536,
          "metric": "cosine",
          "index_type": "hnsw",
          "index_params": {
            "ef_construct": 128,
            "m": 16
          }
        },
        "context_embedding": {
          "dimension": 1536,
          "metric": "cosine",
          "index_type": "hnsw",
          "index_params": {
            "ef_construct": 128,
            "m": 16
          }
        }
      }
    },
    "indexes": [
      {
        "name": "idx_vector_action_embedding",
        "type": "hnsw",
        "field": "action_embedding",
        "parameters": {
          "ef_construct": 128,
          "m": 16,
          "metric": "cosine"
        }
      },
      {
        "name": "idx_vector_context_embedding",
        "type": "hnsw",
        "field": "context_embedding",
        "parameters": {
          "ef_construct": 128,
          "m": 16,
          "metric": "cosine"
        }
      }
    ],
    "validation_rules": {
      "interaction_id": [
        {
          "type": "string",
          "min_length": 1,
          "max_length": 255
        }
      ],
      "user_id": [
        {
          "type": "string",
          "min_length": 1,
          "max_length": 255
        }
      ],
      "entity_id": [
        {
          "type": "string",
          "min_length": 1,
          "max_length": 255
        }
      ],
      "action": [
        {
          "type": "string",
          "min_length": 1,
          "max_length": 255
        }
      ],
      "context": [
        {
          "type": "object"
        }
      ],
      "timestamp": [
        {
          "type": "string",
          "format": "date-time"
        }
      ]
    },
    "access_patterns": []
  }
}

## Relationships

{
  "users": {
    "user_id": {
      "type": "belongs_to",
      "target_collection": "users",
      "target_field": "user_id",
      "cascade_delete": false,
      "indexed": true
    },
    "users_ids": {
      "type": "has_many",
      "target_collection": "users",
      "target_field": "user_id",
      "virtual": true
    },
    "interactions_ids": {
      "type": "has_many",
      "target_collection": "interactions",
      "target_field": "user_id",
      "virtual": true
    }
  },
  "interactions": {
    "interaction_id": {
      "type": "belongs_to",
      "target_collection": "interactions",
      "target_field": "interaction_id",
      "cascade_delete": false,
      "indexed": true
    },
    "user_id": {
      "type": "belongs_to",
      "target_collection": "users",
      "target_field": "user_id",
      "cascade_delete": false,
      "indexed": true
    },
    "interactions_ids": {
      "type": "has_many",
      "target_collection": "interactions",
      "target_field": "interaction_id",
      "virtual": true
    }
  }
}

## Indexes

{
  "entities": [
    {
      "name": "idx_entities_entity_id",
      "fields": [
        "entity_id"
      ],
      "type": "btree",
      "unique": false
    },
    {
      "name": "idx_entities_status",
      "fields": [
        "status"
      ],
      "type": "btree",
      "unique": false
    },
    {
      "name": "idx_entities_category",
      "fields": [
        "category"
      ],
      "type": "btree",
      "unique": false
    },
    {
      "name": "idx_entities_created_at",
      "fields": [
        "created_at"
      ],
      "type": "btree",
      "unique": false
    },
    {
      "name": "idx_entities_updated_at",
      "fields": [
        "updated_at"
      ],
      "type": "btree",
      "unique": false
    },
    {
      "name": "idx_entities_fulltext",
      "fields": [
        "entity_id",
        "name",
        "description"
      ],
      "type": "gin",
      "unique": false
    }
  ],
  "users": [
    {
      "name": "idx_users_user_id",
      "fields": [
        "user_id"
      ],
      "type": "btree",
      "unique": true
    },
    {
      "name": "idx_users_email",
      "fields": [
        "email"
      ],
      "type": "btree",
      "unique": true
    },
    {
      "name": "idx_users_created_at",
      "fields": [
        "created_at"
      ],
      "type": "btree",
      "unique": false
    },
    {
      "name": "idx_users_fulltext",
      "fields": [
        "user_id",
        "email",
        "name"
      ],
      "type": "gin",
      "unique": false
    }
  ],
  "interactions": [
    {
      "name": "idx_interactions_interaction_id",
      "fields": [
        "interaction_id"
      ],
      "type": "btree",
      "unique": true
    },
    {
      "name": "idx_interactions_user_id",
      "fields": [
        "user_id"
      ],
      "type": "btree",
      "unique": false
    },
    {
      "name": "idx_interactions_entity_id",
      "fields": [
        "entity_id"
      ],
      "type": "btree",
      "unique": false
    },
    {
      "name": "idx_interactions_timestamp",
      "fields": [
        "timestamp"
      ],
      "type": "btree",
      "unique": false
    },
    {
      "name": "idx_interactions_composite",
      "fields": [
        "interaction_id",
        "user_id"
      ],
      "type": "btree",
      "unique": false
    },
    {
      "name": "idx_interactions_fulltext",
      "fields": [
        "interaction_id",
        "user_id",
        "entity_id"
      ],
      "type": "gin",
      "unique": false
    }
  ]
}

## Schema Details

```json
{
  "database_name": "taskmaster_pro",
  "database_type": "zerodb",
  "description": "AI-native TaskMaster Pro built on ZeroDB",
  "collections": {
    "entities": {
      "name": "entities",
      "description": "Primary application entities",
      "vector_fields": [
        "content_embedding",
        "metadata_embedding"
      ],
      "metadata_schema": {
        "entity_id": "string",
        "name": "string",
        "description": "text",
        "status": "string",
        "category": "string",
        "tags": "array",
        "created_at": "timestamp",
        "updated_at": "timestamp"
      },
      "vector_config": {
        "enabled": true,
        "dimension": 1536,
        "metric": "cosine",
        "fields": {
          "content_embedding": {
            "dimension": 1536,
            "metric": "cosine",
            "index_type": "hnsw",
            "index_params": {
              "ef_construct": 128,
              "m": 16
            }
          },
          "metadata_embedding": {
            "dimension": 1536,
            "metric": "cosine",
            "index_type": "hnsw",
            "index_params": {
              "ef_construct": 128,
              "m": 16
            }
          }
        }
      },
      "indexes": [
        {
          "name": "idx_vector_content_embedding",
          "type": "hnsw",
          "field": "content_embedding",
          "parameters": {
            "ef_construct": 128,
            "m": 16,
            "metric": "cosine"
          }
        },
        {
          "name": "idx_vector_metadata_embedding",
          "type": "hnsw",
          "field": "metadata_embedding",
          "parameters": {
            "ef_construct": 128,
            "m": 16,
            "metric": "cosine"
          }
        }
      ],
      "validation_rules": {
        "entity_id": [
          {
            "type": "string",
            "min_length": 1,
            "max_length": 255
          }
        ],
        "name": [
          {
            "type": "string",
            "min_length": 1,
            "max_length": 255
          }
        ],
        "description": [
          {
            "type": "string",
            "min_length": 1,
            "max_length": 10000
          }
        ],
        "status": [
          {
            "type": "string",
            "min_length": 1,
            "max_length": 255
          }
        ],
        "category": [
          {
            "type": "string",
            "min_length": 1,
            "max_length": 255
          }
        ],
        "tags": [
          {
            "type": "array",
            "max_items": 100
          }
        ],
        "created_at": [
          {
            "type": "string",
            "format": "date-time"
          }
        ],
        "updated_at": [
          {
            "type": "string",
            "format": "date-time"
          }
        ]
      },
      "access_patterns": []
    },
    "users": {
      "name": "users",
      "description": "Application users",
      "vector_fields": [
        "profile_embedding"
      ],
      "metadata_schema": {
        "user_id": "string",
        "email": "string",
        "name": "string",
        "role": "string",
        "created_at": "timestamp",
        "last_active": "timestamp"
      },
      "vector_config": {
        "enabled": true,
        "dimension": 1536,
        "metric": "cosine",
        "fields": {
          "profile_embedding": {
            "dimension": 1536,
            "metric": "cosine",
            "index_type": "hnsw",
            "index_params": {
              "ef_construct": 128,
              "m": 16
            }
          }
        }
      },
      "indexes": [
        {
          "name": "idx_vector_profile_embedding",
          "type": "hnsw",
          "field": "profile_embedding",
          "parameters": {
            "ef_construct": 128,
            "m": 16,
            "metric": "cosine"
          }
        }
      ],
      "validation_rules": {
        "user_id": [
          {
            "type": "string",
            "min_length": 1,
            "max_length": 255
          }
        ],
        "email": [
          {
            "type": "string",
            "min_length": 1,
            "max_length": 255
          },
          {
            "format": "email"
          }
        ],
        "name": [
          {
            "type": "string",
            "min_length": 1,
            "max_length": 255
          }
        ],
        "role": [
          {
            "type": "string",
            "min_length": 1,
            "max_length": 255
          }
        ],
        "created_at": [
          {
            "type": "string",
            "format": "date-time"
          }
        ],
        "last_active": [
          {
            "type": "string",
            "format": "date-time"
          }
        ]
      },
      "access_patterns": [
        {
          "name": "user_lookup",
          "query": {
            "email": "?"
          },
          "frequency": "high"
        },
        {
          "name": "active_users",
          "query": {
            "status": "active"
          },
          "frequency": "medium"
        },
        {
          "name": "recent_users",
          "sort": {
            "created_at": -1
          },
          "frequency": "low"
        }
      ]
    },
    "interactions": {
      "name": "interactions",
      "description": "User interactions and activities",
      "vector_fields": [
        "action_embedding",
        "context_embedding"
      ],
      "metadata_schema": {
        "interaction_id": "string",
        "user_id": "string",
        "entity_id": "string",
        "action": "string",
        "context": "json",
        "timestamp": "timestamp"
      },
      "vector_config": {
        "enabled": true,
        "dimension": 1536,
        "metric": "cosine",
        "fields": {
          "action_embedding": {
            "dimension": 1536,
            "metric": "cosine",
            "index_type": "hnsw",
            "index_params": {
              "ef_construct": 128,
              "m": 16
            }
          },
          "context_embedding": {
            "dimension": 1536,
            "metric": "cosine",
            "index_type": "hnsw",
            "index_params": {
              "ef_construct": 128,
              "m": 16
            }
          }
        }
      },
      "indexes": [
        {
          "name": "idx_vector_action_embedding",
          "type": "hnsw",
          "field": "action_embedding",
          "parameters": {
            "ef_construct": 128,
            "m": 16,
            "metric": "cosine"
          }
        },
        {
          "name": "idx_vector_context_embedding",
          "type": "hnsw",
          "field": "context_embedding",
          "parameters": {
            "ef_construct": 128,
            "m": 16,
            "metric": "cosine"
          }
        }
      ],
      "validation_rules": {
        "interaction_id": [
          {
            "type": "string",
            "min_length": 1,
            "max_length": 255
          }
        ],
        "user_id": [
          {
            "type": "string",
            "min_length": 1,
            "max_length": 255
          }
        ],
        "entity_id": [
          {
            "type": "string",
            "min_length": 1,
            "max_length": 255
          }
        ],
        "action": [
          {
            "type": "string",
            "min_length": 1,
            "max_length": 255
          }
        ],
        "context": [
          {
            "type": "object"
          }
        ],
        "timestamp": [
          {
            "type": "string",
            "format": "date-time"
          }
        ]
      },
      "access_patterns": []
    }
  },
  "relationships": {
    "users": {
      "user_id": {
        "type": "belongs_to",
        "target_collection": "users",
        "target_field": "user_id",
        "cascade_delete": false,
        "indexed": true
      },
      "users_ids": {
        "type": "has_many",
        "target_collection": "users",
        "target_field": "user_id",
        "virtual": true
      },
      "interactions_ids": {
        "type": "has_many",
        "target_collection": "interactions",
        "target_field": "user_id",
        "virtual": true
      }
    },
    "interactions": {
      "interaction_id": {
        "type": "belongs_to",
        "target_collection": "interactions",
        "target_field": "interaction_id",
        "cascade_delete": false,
        "indexed": true
      },
      "user_id": {
        "type": "belongs_to",
        "target_collection": "users",
        "target_field": "user_id",
        "cascade_delete": false,
        "indexed": true
      },
      "interactions_ids": {
        "type": "has_many",
        "target_collection": "interactions",
        "target_field": "interaction_id",
        "virtual": true
      }
    }
  },
  "indexes": {
    "entities": [
      {
        "name": "idx_entities_entity_id",
        "fields": [
          "entity_id"
        ],
        "type": "btree",
        "unique": false
      },
      {
        "name": "idx_entities_status",
        "fields": [
          "status"
        ],
        "type": "btree",
        "unique": false
      },
      {
        "name": "idx_entities_category",
        "fields": [
          "category"
        ],
        "type": "btree",
        "unique": false
      },
      {
        "name": "idx_entities_created_at",
        "fields": [
          "created_at"
        ],
        "type": "btree",
        "unique": false
      },
      {
        "name": "idx_entities_updated_at",
        "fields": [
          "updated_at"
        ],
        "type": "btree",
        "unique": false
      },
      {
        "name": "idx_entities_fulltext",
        "fields": [
          "entity_id",
          "name",
          "description"
        ],
        "type": "gin",
        "unique": false
      }
    ],
    "users": [
      {
        "name": "idx_users_user_id",
        "fields": [
          "user_id"
        ],
        "type": "btree",
        "unique": true
      },
      {
        "name": "idx_users_email",
        "fields": [
          "email"
        ],
        "type": "btree",
        "unique": true
      },
      {
        "name": "idx_users_created_at",
        "fields": [
          "created_at"
        ],
        "type": "btree",
        "unique": false
      },
      {
        "name": "idx_users_fulltext",
        "fields": [
          "user_id",
          "email",
          "name"
        ],
        "type": "gin",
        "unique": false
      }
    ],
    "interactions": [
      {
        "name": "idx_interactions_interaction_id",
        "fields": [
          "interaction_id"
        ],
        "type": "btree",
        "unique": true
      },
      {
        "name": "idx_interactions_user_id",
        "fields": [
          "user_id"
        ],
        "type": "btree",
        "unique": false
      },
      {
        "name": "idx_interactions_entity_id",
        "fields": [
          "entity_id"
        ],
        "type": "btree",
        "unique": false
      },
      {
        "name": "idx_interactions_timestamp",
        "fields": [
          "timestamp"
        ],
        "type": "btree",
        "unique": false
      },
      {
        "name": "idx_interactions_composite",
        "fields": [
          "interaction_id",
          "user_id"
        ],
        "type": "btree",
        "unique": false
      },
      {
        "name": "idx_interactions_fulltext",
        "fields": [
          "interaction_id",
          "user_id",
          "entity_id"
        ],
        "type": "gin",
        "unique": false
      }
    ]
  },
  "security_rules": {
    "authentication_required": true,
    "default_access": "deny",
    "role_based_access": true,
    "collection_rules": {
      "entities": {
        "read": [
          "authenticated"
        ],
        "write": [
          "authenticated"
        ],
        "delete": [
          "owner",
          "admin"
        ]
      },
      "users": {
        "read": [
          "self",
          "admin"
        ],
        "write": [
          "self",
          "admin"
        ],
        "delete": [
          "admin"
        ]
      },
      "interactions": {
        "read": [
          "authenticated"
        ],
        "write": [
          "authenticated"
        ],
        "delete": [
          "owner",
          "admin"
        ]
      }
    }
  },
  "api_mappings": {
    "endpoints": {
      "GET /api/v1/entities": {
        "collection": "entities",
        "method": "GET",
        "path": "/api/v1/entities",
        "operation": "list"
      },
      "POST /api/v1/entities": {
        "collection": "entities",
        "method": "POST",
        "path": "/api/v1/entities",
        "operation": "create"
      },
      "GET /api/v1/users": {
        "collection": "users",
        "method": "GET",
        "path": "/api/v1/users",
        "operation": "list"
      }
    },
    "collection_endpoints": {
      "entities": {
        "list": "GET /api/v1/entities",
        "create": "POST /api/v1/entities",
        "read": "GET /api/v1/entities/{id}",
        "update": "PUT /api/v1/entities/{id}",
        "delete": "DELETE /api/v1/entities/{id}",
        "search": "GET /api/v1/entities/search"
      },
      "users": {
        "list": "GET /api/v1/users",
        "create": "POST /api/v1/users",
        "read": "GET /api/v1/users/{id}",
        "update": "PUT /api/v1/users/{id}",
        "delete": "DELETE /api/v1/users/{id}",
        "search": "GET /api/v1/users/search"
      },
      "interactions": {
        "list": "GET /api/v1/interactions",
        "create": "POST /api/v1/interactions",
        "read": "GET /api/v1/interactions/{id}",
        "update": "PUT /api/v1/interactions/{id}",
        "delete": "DELETE /api/v1/interactions/{id}",
        "search": "GET /api/v1/interactions/search"
      }
    }
  },
  "created_at": "2025-12-06T04:20:59.844414",
  "version": "1.0.0"
}
```
