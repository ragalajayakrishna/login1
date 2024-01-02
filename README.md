from django.shortcuts import render,redirect
from django.http import HttpResponse
from django.http import HttpResponseRedirect
from django.shortcuts import get_object_or_404
from.models import Emp

# def nellore(request):

#       if request.method =='POST':
#         empname=request.POST['name']
#         empemail=request.POST['email']
#         empmsg=request.POST['message']
#         data=Emp(Emp_name=empname,Emp_email=empemail,Emp_message=empmsg)
#         data.save()
        
#         return render(request,'index.html')
#       return render (request,'index.html')

# def Display(request):
#     if request.method == 'GET':
#         a=Emp.objects.all()
#         return render(request,"data.html",{"data":a})
#     elif request.method == 'POST':
#         return HttpResponseRedirect(request.path)
    
# def Update_Record(request):
#     if request.method == 'POST':
        
#         id = request.POST['id']
#         name = request.POST['name']
#         email = request.POST['email']
#         message= request.POST['message']
        
#         b = get_object_or_404(Emp,pk=id)
        
        
#         b.Emp_name = name
#         b.Emp_email = email
#         b.Emp_message = message
#         b.save()
#         return HttpResponseRedirect ("/data/")


# def Delete_record(request,id):
#     if request.method == 'POST':
#         a=Emp.objects.get(pk=id)
#         a.delete()
#         return redirect('/data/')







def SignUp(request):
    if request.method == 'POST':
       
        uname = request.POST['username']
        email = request.POST['email']
        pass1 = request.POST['password1']
        pass2 = request.POST['password2']
        
        

        if pass1!=pass2:
            return HttpResponse('Password not matched')
        
        elif Emp.objects.filter(Emp_name=uname).exists():
            return HttpResponse('username already exits!!!')
        else:
            my_user =Emp(Emp_name = uname,Emp_email = email,Emp_pass1=pass1,Emp_pass2 = pass2)
            my_user.save()
        
            return redirect('signup')
        
    return render(request,'signup.html')


def LogIn(request):
    if request.method == "POST":
        usernames = request.POST['username']
        pass1 = request.POST['pass']
        
        user = get_object_or_404(Emp,Emp_name = usernames)

        if user .Emp_pass1 == pass1:
                return redirect('home',user_id=user.id)
        
        else:
            return HttpResponse('Username and password does not exits!!!')
    
    
def Home(request,user_id):
    return render(request,'home.html',{'user_id':user_id})

def LogOut(request):
    return redirect('signup')

        

def Delete(request,user_id):
        a = get_object_or_404(Emp, id=user_id)
        a.delete()
        return redirect('signup')
