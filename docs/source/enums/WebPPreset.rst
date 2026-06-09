***************
WebPPreset enum
***************

.. py:class:: WebPPreset

    Represents WebP encoder presets. WebPPreset is an enum with constants that are derived from the underlying libwebp. These are:

        * WEBP_PRESET_DEFAULT
        * WEBP_PRESET_PICTURE
        * WEBP_PRESET_PHOTO
        * WEBP_PRESET_DRAWING
        * WEBP_PRESET_ICON
        * WEBP_PRESET_TEXT
    
    WebPPreset constants are used when creating a :py:class:`WebPConfig`. The presets set specific parameters in the underlying libwebp's WebPConfig used to encode the image. Which parameters each WebPPreset constant represents are documented below.


    .. py:attribute:: DEFAULT

        Sets no special parameters to the encoder's configuration.

        .. tip::
            This is the default preset when creating a :py:class:`WebPConfig`., so there is no need to specify ``WebPPreset.DEFAULT`` when instantiating a :py:class:`WebPConfig`.

    .. py:attribute:: PICTURE
        
        Sets the following parameters for the encoder's configuration:

            * ``sns_strength = 80``
            * ``filter_sharpness = 4``
            * ``filter_strength = 35``
            * ``fipreprocessing &= ~2``

        .. note::
        
            This value of``preprocessing`` disables dithering.

    .. py:attribute:: PHOTO
        
        Sets the following parameters for the encoder's configuration:

            * ``sns_strength = 80``
            * ``filter_sharpness = 3``
            * ``filter_strength = 30``
            * ``preprocessing |= 2``

    .. py:attribute:: DRAWING
        
        Sets the following parameters for the encoder's configuration:

              * ``sns_strength = 25``
              * ``filter_sharpness = 6``
              * ``filter_strength = 10``

    .. py:attribute:: ICON
        
        Sets the following parameters for the encoder's configuration:

              * ``sns_strength = 0``
              * ``filter_strength = 0``
              * ``preprocessing &= ~2`` 

        .. note::
            
            This value of ``filter_strength`` disables filtering in order to retain sharpness.
            This value of``preprocessing`` disables dithering.

    .. py:attribute:: TEXT
        
        Sets the following parameters for the encoder's configuration:

              * ``sns_strength = 0
              * ``filter_strength = 0``
              * ``preprocessing &= ~2``
              * ``segments = 2``

        .. note::
            
            This value of ``filter_strength`` disables filtering in order to retain sharpness.
            This value of``preprocessing`` disables dithering.


========
Examples
========

Encode an image with the WebPPreset.PHOTO preset
------------------------------------------------

.. code-block:: python

    import os

    import PIL
    import webp
    
    MESSAGE = '''Enter name of file to encode as\
     a webp with the photo preset: '''

    infilename =  input(MESSAGE)
    outfilename = os.path.splitext(infilename)[0] + '-photo.webp'

    with PIL.Image.open(infilename, 'r') as image:
		pic = webp.WebPPicture.from_pil(image)

    # The part where we specify the preset
    config = webp.WebPConfig.new(preset=webp.WebPPreset.PHOTO)

    # The part where we use our config with our preset
    data = pic.encode(config)
	
    with open(outfilename, 'wb') as f:
  		f.write(data.buffer())

.. tip::

	Recall that :py:class:`WebPConfig`'s constructor method is :py:func:`WebPConfig.new`.


Selectively encode an image that has text
-----------------------------------------

.. code-block:: python

    import os

    import PIL
    import webp
    import pytesseract

    MESSAGE = '''Enter name of file to convert to webp: '''

    infilename =  input(MESSAGE)
    outfilename = os.path.splitext(infilename)[0] + '.webp'

    with PIL.Image.open(infilename, 'r') as image:
        image_text = pytesseract.image_to_string(image)
        pic = webp.WebPPicture.from_pil(image)

    # Check if the image has text
    if not image_text == '':
        print('TEXT')
        preset = webp.WebPPreset.TEXT
    else:
        # Assume that an image with no text is actually a photo
        print('PHOTO')
        preset = webp.WebPPreset.PHOTO

    # Use a low quality so we can clearly see the effect of the text preset
    config = webp.WebPConfig.new(preset=preset, quality=30)

    # Now encode our image and write our file
    data = pic.encode(config)
    with open(outfilename, 'wb') as f:
        f.write(data.buffer())


.. admonition:: Exercise
    
    Modify this program so that for images with text, it outputs both an image with the TEXT preset but also one with the PHOTO preset. Then,  compare how text looks with each preset. Make sure to keep the low quality to make comparisons fair and easy.

.. tip::
	
	Make sure you have tesseract installed on your system.

Ask the user what kind of image they want
-----------------------------------------
TODO
.. code-block:: python
    
    # Ask the user what kind of image they want
    import webp

