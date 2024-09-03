My static assets(Bootsrapp icons, Fontawsome, images..etc….) where not showing when I launched my Net8.0 app using IIS

Here are the steps I took to fix it.

A) many others where saying look for a 404 error under network on your browser, I did but did not find a 404 error, instead I 
   I found a ' 'cache-control' header is missing or empty' under issues on the network tab. meaning there was nothing wrong with the 
   paths to my static files or npm inside node modules, the issue was with the headers.

B) go to your Startup.cs or program.cs or whichever you use under 'public void Configure(IApplicationBuilder app, IWebHostEnvironment env)'
   go to 'app.UseStaticFiles and configure it for the Headers like this:
  

                     app.UseStaticFiles(new StaticFileOptions
                    {
                        OnPrepareResponse = ctx =>
                        {
                            ctx.Context.Response.Headers.Append("Cache-Control", "public,max-age=600");
                        }
                    });

 C) Next Add Namespace: Ensure you have 'using Microsoft.AspNetCore.Http;' at the top of your Startup.cs,or program.cs file to avoid issues when using the Append method.

D) Finally, clear your browser cache and then launch your app and that is how I solved it
