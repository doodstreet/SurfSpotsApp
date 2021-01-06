
<h2>Python Live Project Intership</h2>
   <p>This was an intership experience with Prosper I.T. Consulting. This project was a great experience for me. We used Slack
 for team communication and Azure DevOps for project management during this two week sprint. I created a Surf Spots App as part of a much larger 
 Hobby Tracker website and created a database for the App to connect with and log favorite surf spots with full CRUD functionality.
 This included using Python3 in virtual environments, Pycharm as the IDE and Git as version control. The parameters of this project were to create a list
 of something of personal interest with (CRUD) the ablity to Create, Read, Update, Delete and see details plus apply CSS styling and app functionality.<br>
   I created a "Surf Spots" app to save a list of all my favorite surf spots with details about each spot and the ablitiy to add, change and delete
 content.<br>
 Here is a code example of my views for this project:</p>
 <br>

 Home page ------------
```
def surfApp_home(request):
    # get the list of spotNames to show your list of favorite spots on the home page
    spots = SurfSpot.objects.all()
    context = {'spots': spots}
 link to home page and display requested content
    return render(request, 'surfApp/surfApp_home.html', context)
```    
 Add Spot page-----use model to create a form and post to database
```
def surfApp_addSpot(request):
    # gets requested form if exists
    form = SurfSpotForm(request.POST or None)
    if form.is_valid():
        #  checks that form has the required entries filled in and if
        #  valid with no errors, saves to the database
        form.save()
 when done with saving form, this redirects to the index (list of spots) page
        return redirect('SpotIndex')

    else:
        # if not saved, print errors to the terminal
        print(form.errors)
        form = SurfSpotForm()
    context = {'form': form, }
    return render(request, 'surfApp/surfApp_addSpot.html',  context)
```
 Index page---- use index function to get database objects and render them on the index page.
```
def surfApp_index(request):
    # gets all posts from database
    spots = SurfSpot.objects.all()
 print to terminal to make sure data is populating
    print(spots)
 creates dictionary for items in the database and passes the args to the page
    context = {'spots': spots, }
    return render(request, 'surfApp/surfApp_index.html', context)
```
  Details page---- use primary key to get details from that item
```
def surfApp_details(request, pk):
    # gets a single instance of the SurfSpot model object from the database
    spot = get_object_or_404(SurfSpot, pk=pk)
    context = {'spot': spot, }
    return render(request, 'surfApp/surfApp_details.html', context)
```
 Update page (edit)----
```
def surfApp_update(request, pk):
    # means Django expects an integer value and will transfer it to a view as a variable called pk
    pk = int(pk)
    spot = get_object_or_404(SurfSpot, pk=pk)
 HTTP method POST-- finds the form that was submitted by a user
    if request.method == 'POST':
        # and we can find that forms filled out answers with the request.POST
        form = SurfSpotForm(request.POST, instance=spot)
        if form.is_valid():
            form.save()
            return redirect('SpotIndex')
        else:
            print(form.errors)
    form = SurfSpotForm(instance=spot)
    context = {'form': form, }
    return render(request, 'surfApp/surfApp_update.html', context)
```
 Delete -- from details page, get instance of spot
```
def surfApp_delete(request, pk):
    pk = int(pk)
    spot = get_object_or_404(SurfSpot, pk=pk)
    context = {'spot': spot, }
    return render(request, 'surfApp/surfApp_delete.html', context)
```
 Confirm delete-- renders delete page for confirmation of action
```
def confirm_delete(request, pk):
    pk = int(pk)
    spot = get_object_or_404(SurfSpot, pk=pk)
    spot.delete()
    return redirect('SurfSpotsHome')
```
<strong>And my urlpatterns for this app:</strong>
```
urlpatterns = [
    path('', views.surfApp_home, name='SurfSpotsHome'),
    path('AddSpot', views.surfApp_addSpot, name='AddSpot'),
    path('SpotIndex', views.surfApp_index, name='SpotIndex'),
    path('<int:pk>/SpotDetails', views.surfApp_details, name='SpotDetails'),
    path('<int:pk>/UpdateSpot', views.surfApp_update, name='UpdateSpot'),
    path('<int:pk>/DeleteSpot', views.surfApp_delete, name='DeleteSpot'),
    path('<int:pk>/ConfirmDelete', views.confirm_delete, name='ConfirmDelete'),
]
```
<h2>Conclusion</h2>
