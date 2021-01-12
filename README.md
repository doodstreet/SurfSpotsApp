
<h2>Surf Spots Python Internship App</h2>
  Hi, my name is Dave Street. I completed Software Developer Boot Camp and earned my certification from The Tech Academy. This was an intership experience  with Prosper I.T. Consulting. This project was a great experience for me using Agile/Scrum methodologies during this two week sprint. I created a Surf Spots  App as part of a much larger Hobby Tracker website and created a database for the App to connect with and log favorite surf spots with full CRUD functionality.
This included using Python3 in virtual environments, Pycharm as the IDE and Git as version control..<br>
<br>
Here is a code example of my views for this project:
<br><br>
 Home page ------------
 ![SurfSpotsAppHome](https://user-images.githubusercontent.com/68976585/103727518-50896a80-4f90-11eb-89ce-df33eeeb3dde.png)
<br>
This is the Home page View request that opens the home page as well as displays an current list of saved spots on it. 
```
def surfApp_home(request):
    # get the list of spotNames to show your list of favorite spots on the home page
    spots = SurfSpot.objects.all()
    context = {'spots': spots}
 link to home page and display requested content
    return render(request, 'surfApp/surfApp_home.html', context)
```    
 Add Spot page-----use model to create a form and post to database
 ![SurfSpotAppCreate](https://user-images.githubusercontent.com/68976585/103727616-8e868e80-4f90-11eb-91bc-8ca284f71b18.png)
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
 ![SurfSpotAppIndex](https://user-images.githubusercontent.com/68976585/103727699-c1308700-4f90-11eb-89da-e3565d74c35f.png)
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
  ![SurfSpotAppDetails](https://user-images.githubusercontent.com/68976585/103727727-d5748400-4f90-11eb-8a85-52037cb052d5.png)
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
 ![SurfSpotsAppDelete](https://user-images.githubusercontent.com/68976585/103727794-0b196d00-4f91-11eb-8b98-6f1a1667c5f8.png)
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
