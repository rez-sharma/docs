.. meta::
   :description: UP42 data blocks: GeoTIFF Custom data block description
   :keywords: GeoTIFF, custom, data, tasking

.. _geotiff-custom-data-block:

Import Data (GeoTIFF)
=====================
For more information, please read the `block description <https://marketplace.up42.com/block/eed51bcb-c7cc-4084-b518-6c59f46b48c8>`_.

Block type: ``DATA``

This block enables using GeoTIFF data, stored in a bucket on *Google Cloud Storage (GCS)*
or *Amazon Web Services (AWS)*, in a workflow on UP42.

Within the bucket, the user can select specific images (via the filenames) or search by
location and time. The search can also be limited to a subfolder in the bucket via the
`prefix` parameter.
The block outputs the scene data and an automatically created `data.json` file with the scene metadata.

The block can connect to any processing block which expects output from the SPOT or Pléiades sensor or is
sensor-agnostic (i.e. does not expect any specific sensor).

.. tip::

    In order to access the bucket, the access credentials need to be provided via UP42 environment variables.
    For AWS, provide the `AWS_ACCESS_KEY_ID`, `AWS_SECRET_ACCESS_KEY` as environment variables. No bucket region setting is required.
    For GCS, provide the full json string of the Google Application Credentials json as the `GOOGLE_KEY_STRING` environment variable. Example:

    .. figure:: ../../_assets/env_variables.png
       :align: center
       :scale: 50 %
       :alt: Environment variables


Supported parameters
--------------------

For more information, please read the section :ref:`Data source query filters  <filters>`.

* ``cloud_provider``: The cloud storage provider of the bucket, either ``gcs`` or ``aws``.
* ``bucket_name``: The bucket name.
* ``prefix``: A file structure prefix to limit the dataset search to a specific subdirectory. Conforms to the gcs & aws prefix structure,
  which excludes the bucket name. E.g. `folder1/folder2/`.
* ``filenames``: An array of GeoTIFF filenames, including suffix. The ``filenames`` filter overrides all other filters, e.g., ``intersects``, ``limit`` and/or ``time``.
* ``time``: A date range to filter scenes on. This range applies to the acquisition date/time of the scenes.
* ``bbox``: The bounding box to use as an AOI. Will return all scenes that intersect with this box. Use only ``box``
  **or** ``intersects``.
* ``intersects``: A GeoJSON geometry to use as an AOI. Will return all scenes that intersect with this geometry. Use
  only ``intersects`` **or** ``bbox``.
* ``contains``: A GeoJSON geometry to use as an AOI. Will return all scenes that completely cover this geometry. Use only ``contains``
  **or** ``intersects`` **or** ``bbox``.
* ``limit``: An integer number of maximum results to return. Omit this to set no limit.


Example queries
---------------

Example query with Google Cloud Storage, using ``filenames`` and ``prefix``:

.. code-block:: javascript

    {
        "geotiff-custom:1": {
            "cloud_provider": "gcs",
            "bucket_name": "geotiff-scenes-data",
            "prefix": "europe/france/",
            "filenames": ["33c03020-2797-4bd4-b3b7-763d4de12754_ms.tif"],
            "intersects": null,
            "time": "2018-01-01T00:00:00+00:00/2020-12-31T23:59:59+00:00",
            "limit": 1
        }
    }

Example query with Amazon Web services, searching via ``time`` & ``aoi``.

.. code-block:: javascript

    {
        "geotiff-custom:1": {
            "cloud_provider": "aws",
            "bucket_name": "geotiff-scenes-data",
            "prefix": null,
            "filenames": null,
            "bbox": [13.351818, 52.501907, 13.379109, 52.510788],
            "time": "2019-01-01T00:00:00+00:00/2020-12-31T23:59:59+00:00",
            "limit": 4
        }
    }

Output format
-------------

The output GeoJSON contains the GeoTIFF file metadata, with the ``up42.data_path`` pointing to the GeoTIFF file.

.. code-block:: javascript

    {
      "type": "FeatureCollection",
      "features": [
        {
          "type": "Feature",
          "filename": "33c03020-2797-4bd4-b3b7-763d4de12754_ms.tif",
          "bbox": [
            -8.826857337352216,
            37.95072101226636,
            -8.804132335571202,
            37.968715633929804
          ],
          "geometry": {
            "type": "Polygon",
            "coordinates": [
              [
                [
                  -8.804132,
                  37.950721
                ],
                [
                  -8.804132,
                  37.968716
                ],
                [
                  -8.826857,
                  37.968716
                ],
                [
                  -8.826857,
                  37.950721
                ],
                [
                  -8.804132,
                  37.950721
                ]
              ]
            ]
          },
          "properties": {
            "driver": "GTiff",
            "dtype": "uint16",
            "nodata": null,
            "width": 711,
            "height": 563,
            "count": 4,
            "crs": "EPSG:4326",
            "transform": [
              3.196202782139787e-05,
              0.0,
              -8.826857337352216,
              0.0,
              -3.1962027821399064e-05,
              37.968715633929804,
              0.0,
              0.0,
              1.0
            ],
            "up42.data_path": "33c03020-2797-4bd4-b3b7-763d4de12754_ms.tif"
          }
        }
      ]
    }
