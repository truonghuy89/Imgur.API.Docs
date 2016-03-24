Quick Start
===========

Get Image
---------

::

        public async Task GetImage()
        {
            try
            {
                var client = new ImgurClient("CLIENT_ID", "CLIENT_SECRET");
                var endpoint = new ImageEndpoint(client);
                var image = await endpoint.GetImageAsync("IMAGE_ID");
                Debug.Write("Image retrieved. Image Url: " + image.Link);
            }
            catch (ImgurException imgurEx)
            {
                Debug.Write("An error occurred getting an image from Imgur.");
                Debug.Write(imgurEx.Message);
            }
        }
        

Get Image (synchronously - not recommended)
---------

::

        public async Task GetImage()
        {
            try
            {
                var client = new ImgurClient("CLIENT_ID", "CLIENT_SECRET");
                var endpoint = new ImageEndpoint(client);
                var task = endpoint.GetImageAsync("IMAGE_ID");
                Task.WaitAll(task);
                var image = task.Result;
                Debug.Write("Image retrieved. Image Url: " + image.Link);
            }
            catch (ImgurException imgurEx)
            {
                Debug.Write("An error occurred getting an image from Imgur.");
                Debug.Write(imgurEx.Message);
            }
        }
        

Upload Image
------------

::

        public async Task UploadImage()
        {
            try
            {
                var client = new ImgurClient("CLIENT_ID", "CLIENT_SECRET");
                var endpoint = new ImageEndpoint(client);
                IImage image;
                using (var fs = new FileStream(@"IMAGE_LOCATION", FileMode.Open))
                {
                    image = await endpoint.UploadImageStreamAsync(fs);
                }
                Debug.Write("Image uploaded. Image Url: " + image.Link);
            }
            catch (ImgurException imgurEx)
            {
                Debug.Write("An error occurred uploading an image to Imgur.");
                Debug.Write(imgurEx.Message);
            }
        }