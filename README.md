# django-db-connection-pool

MySQL & Oracle connection pool backends of of Django, Be based on SQLAlchemy.


### HOW TO USE

#### MySQL

change ``django.db.backends.mysql`` to ``dj_db_conn_pool.backends.mysql``:

    DATABASES = {
        'default': {
            ...
            'ENGINE': 'dj_db_conn_pool.backends.mysql'
            ...
        }
    }

#### Oracle

change ``django.db.backends.oracle`` to ``dj_db_conn_pool.backends.oracle``:

    DATABASES = {
        'default': {
            ...
            'ENGINE': 'dj_db_conn_pool.backends.oracle'
            ...
        }
    }


#### Configuration

you can provide additional options to pass to SQLAlchemy's pool creation, key's name is POOL:

    DATABASES = {
        'default': {
            ...
            'ENGINE': 'dj_db_conn_pool.backends.mysql',
            'POOL' : {
                'pool_size': 10,
                'max_overflow': 10,
                'recycle': 216000
            }
            ...
         }
     }

Here's explanation of these options(copy from SQLAlchemy's Doc):

* **pool_size**: The size of the pool to be maintained,
          defaults to 5. This is the largest number of connections that
          will be kept persistently in the pool. Note that the pool
          begins with no connections; once this number of connections
          is requested, that number of connections will remain.
          ``pool_size`` can be set to 0 to indicate no size limit; to
          disable pooling, use a :class:`~sqlalchemy.pool.NullPool`
          instead.

* **max_overflow**: The maximum overflow size of the
          pool. When the number of checked-out connections reaches the
          size set in pool_size, additional connections will be
          returned up to this limit. When those additional connections
          are returned to the pool, they are disconnected and
          discarded. It follows then that the total number of
          simultaneous connections the pool will allow is pool_size +
          `max_overflow`, and the total number of "sleeping"
          connections the pool will allow is pool_size. `max_overflow`
          can be set to -1 to indicate no overflow limit; no limit
          will be placed on the total number of concurrent
          connections. Defaults to 10.

* **recycle**: If set to a value other than -1, number of
          seconds between connection recycling, which means upon
          checkout, if this timeout is surpassed the connection will be
          closed and replaced with a newly opened connection. Defaults to -1.