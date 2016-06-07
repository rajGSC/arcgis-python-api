
# Batch creation of Groups

This sample notebook automates the task of creating groups in an ArcGIS Portal or ArcGIS Online Org. A similar task can be done for creating or updating users and content.


    from arcgis.gis import *
    from IPython.display import display
    import pandas as pd

The list of groups to be created in read from a csv file, along with the properties and thumbnails to be used for creating the groups. Such a file can be created using Excel.


    groups_df = pd.read_csv('data/groups.csv')


    groups_df[:3]




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>access</th>
      <th>description</th>
      <th>isFav</th>
      <th>isInvitationOnly</th>
      <th>isViewOnly</th>
      <th>phone</th>
      <th>snippet</th>
      <th>sortField</th>
      <th>sortOrder</th>
      <th>tags</th>
      <th>thumbnail</th>
      <th>title</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>public</td>
      <td>This group includes a complete list of basemap...</td>
      <td>False</td>
      <td>True</td>
      <td>True</td>
      <td>null</td>
      <td>Standard basemaps for our organizations.</td>
      <td>title</td>
      <td>asc</td>
      <td>Maps,Base,Basemap,Basemaps</td>
      <td>Basemaps.png</td>
      <td>Basemaps</td>
    </tr>
    <tr>
      <th>1</th>
      <td>org</td>
      <td>This Group contains an inventory of map servic...</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>null</td>
      <td>The authoritative service catalog.</td>
      <td>title</td>
      <td>asc</td>
      <td>Services,Maps,Data</td>
      <td>CentralServices.png</td>
      <td>Central Services</td>
    </tr>
    <tr>
      <th>2</th>
      <td>org</td>
      <td>A group dealing with government and industry a...</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>null</td>
      <td>Regulatory compliance tracking &amp; reporting.</td>
      <td>modified</td>
      <td>asc</td>
      <td>Regulatory,Compliance</td>
      <td>RegCompliance.png</td>
      <td>Compliance</td>
    </tr>
  </tbody>
</table>
</div>



 The thumbnails are extracted from a zip file (Icons.zip) 


    import zipfile
    with zipfile.ZipFile("data/Icons.zip") as z:
        z.extractall("data")

Using Jupyter Notebooks' 'magic functions' that you can call with a command line style syntax, it's easy to change the directory to the folder containing the extracted icons:


    %cd data/Icons

    C:\xc\GitHub\geosaurus\examples\data\Icons
    

The code below reads the csv file line by line, and creates a group in the GIS using the specified parameters and thumbnail:


    import csv
    
    gis = GIS("https://dev04875.esri.com/arcgis", "portaladmin", "portaladmin")
    groups = []
    with open('..\\..\\data\\groups.csv') as csvfile:
        reader = csv.DictReader(csvfile)
        for row in reader:
            group = gis.groups.create_from_dict(row)
            groups.append(group)
            

To verify, can can display the created groups:


    for g in groups:
        display(g)


<div class="9item_container" style="height: auto; overflow: hidden; border: 1px solid #cfcfcf; border-radius: 2px; background: #f6fafa; line-height: 1.21429em; padding: 10px;">
                    <div class="item_left" style="width: 210px; float: left;">
                       <a href='https://dev04875.esri.com/arcgis/home/group.html?id=b0b059fd52464137917118d1bd44be60' target='_blank'>
                        <img src='data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAEAAAABACAYAAACqaXHeAAAAGXRFWHRTb2Z0d2FyZQBBZG9iZSBJbWFnZVJlYWR5ccllPAAAAyRpVFh0WE1MOmNvbS5hZG9iZS54bXAAAAAAADw/eHBhY2tldCBiZWdpbj0i77u/IiBpZD0iVzVNME1wQ2VoaUh6cmVTek5UY3prYzlkIj8+IDx4OnhtcG1ldGEgeG1sbnM6eD0iYWRvYmU6bnM6bWV0YS8iIHg6eG1wdGs9IkFkb2JlIFhNUCBDb3JlIDUuMy1jMDExIDY2LjE0NTY2MSwgMjAxMi8wMi8wNi0xNDo1NjoyNyAgICAgICAgIj4gPHJkZjpSREYgeG1sbnM6cmRmPSJodHRwOi8vd3d3LnczLm9yZy8xOTk5LzAyLzIyLXJkZi1zeW50YXgtbnMjIj4gPHJkZjpEZXNjcmlwdGlvbiByZGY6YWJvdXQ9IiIgeG1sbnM6eG1wPSJodHRwOi8vbnMuYWRvYmUuY29tL3hhcC8xLjAvIiB4bWxuczp4bXBNTT0iaHR0cDovL25zLmFkb2JlLmNvbS94YXAvMS4wL21tLyIgeG1sbnM6c3RSZWY9Imh0dHA6Ly9ucy5hZG9iZS5jb20veGFwLzEuMC9zVHlwZS9SZXNvdXJjZVJlZiMiIHhtcDpDcmVhdG9yVG9vbD0iQWRvYmUgUGhvdG9zaG9wIENTNiAoTWFjaW50b3NoKSIgeG1wTU06SW5zdGFuY2VJRD0ieG1wLmlpZDo1MDM2MDYxQzc5NzcxMUUyODZGRkEwQ0EzMUFFRUU5NCIgeG1wTU06RG9jdW1lbnRJRD0ieG1wLmRpZDo1MDM2MDYxRDc5NzcxMUUyODZGRkEwQ0EzMUFFRUU5NCI+IDx4bXBNTTpEZXJpdmVkRnJvbSBzdFJlZjppbnN0YW5jZUlEPSJ4bXAuaWlkOjY2NEFCNkRGNzk2RTExRTI4NkZGQTBDQTMxQUVFRTk0IiBzdFJlZjpkb2N1bWVudElEPSJ4bXAuZGlkOjY2NEFCNkUwNzk2RTExRTI4NkZGQTBDQTMxQUVFRTk0Ii8+IDwvcmRmOkRlc2NyaXB0aW9uPiA8L3JkZjpSREY+IDwveDp4bXBtZXRhPiA8P3hwYWNrZXQgZW5kPSJyIj8+a7TrjwAAChZJREFUeNrsW0ts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' ' class="itemThumbnail">
                       </a>
                    </div>
        
                    <div class="item_right" style="float: none; width: auto; overflow: hidden;">
                        <a href='https://dev04875.esri.com/arcgis/home/group.html?id=b0b059fd52464137917118d1bd44be60' target='_blank'><b>Basemaps</b>
                        </a>
                        <br>
                        <br><b>Summary</b>: Standard basemaps for our organizations.
                        <br><b>Description</b>: This group includes a complete list of basemaps for our organization. They provide relevant and consistent geographic context across all city departments; and are essential building blocks for the local government's desktop, mobile and web applications.  
                        <br><b>Owner</b>: portaladmin
                        <br><b>Created</b>: June 04, 2016
                        
                    </div>
                </div>
                



<div class="9item_container" style="height: auto; overflow: hidden; border: 1px solid #cfcfcf; border-radius: 2px; background: #f6fafa; line-height: 1.21429em; padding: 10px;">
                    <div class="item_left" style="width: 210px; float: left;">
                       <a href='https://dev04875.esri.com/arcgis/home/group.html?id=6248d6d74ebf481aa2fe293654894f5a' target='_blank'>
                        <img src='data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAEAAAABACAYAAACqaXHeAAAAGXRFWHRTb2Z0d2FyZQBBZG9iZSBJbWFnZVJlYWR5ccllPAAAAyRpVFh0WE1MOmNvbS5hZG9iZS54bXAAAAAAADw/eHBhY2tldCBiZWdpbj0i77u/IiBpZD0iVzVNME1wQ2VoaUh6cmVTek5UY3prYzlkIj8+IDx4OnhtcG1ldGEgeG1sbnM6eD0iYWRvYmU6bnM6bWV0YS8iIHg6eG1wdGs9IkFkb2JlIFhNUCBDb3JlIDUuMy1jMDExIDY2LjE0NTY2MSwgMjAxMi8wMi8wNi0xNDo1NjoyNyAgICAgICAgIj4gPHJkZjpSREYgeG1sbnM6cmRmPSJodHRwOi8vd3d3LnczLm9yZy8xOTk5LzAyLzIyLXJkZi1zeW50YXgtbnMjIj4gPHJkZjpEZXNjcmlwdGlvbiByZGY6YWJvdXQ9IiIgeG1sbnM6eG1wPSJodHRwOi8vbnMuYWRvYmUuY29tL3hhcC8xLjAvIiB4bWxuczp4bXBNTT0iaHR0cDovL25zLmFkb2JlLmNvbS94YXAvMS4wL21tLyIgeG1sbnM6c3RSZWY9Imh0dHA6Ly9ucy5hZG9iZS5jb20veGFwLzEuMC9zVHlwZS9SZXNvdXJjZVJlZiMiIHhtcDpDcmVhdG9yVG9vbD0iQWRvYmUgUGhvdG9zaG9wIENTNiAoTWFjaW50b3NoKSIgeG1wTU06SW5zdGFuY2VJRD0ieG1wLmlpZDo1MjVCNTUwMjc5NzcxMUUyODZGRkEwQ0EzMUFFRUU5NCIgeG1wTU06RG9jdW1lbnRJRD0ieG1wLmRpZDo1MjVCNTUwMzc5NzcxMUUyODZGRkEwQ0EzMUFFRUU5NCI+IDx4bXBNTTpEZXJpdmVkRnJvbSBzdFJlZjppbnN0YW5jZUlEPSJ4bXAuaWlkOjUyNUI1NTAwNzk3NzExRTI4NkZGQTBDQTMxQUVFRTk0IiBzdFJlZjpkb2N1bWVudElEPSJ4bXAuZGlkOjUyNUI1NTAxNzk3NzExRTI4NkZGQTBDQTMxQUVFRTk0Ii8+IDwvcmRmOkRlc2NyaXB0aW9uPiA8L3JkZjpSREY+IDwveDp4bXBtZXRhPiA8P3hwYWNrZXQgZW5kPSJyIj8+LVihQQAAAwlJREFUeNpi/P//P8NIBkwMIxywwBiNza0CQGo9EDsMcz8fAOLAhrqaD6DUz4Iksd5AX88BiIe17y9cvOQAxKCIdkTPAsPe8yAA9aNDQ1OLwoguA4BZ4AFKGYAOJCQkhpWHX7x4QV4tcOXKlSHraWLczkSMAUMxEIh1O94A0NHRQaGHEiDW7UzEGjQUATFuH/EtwdEAGA2A0c7QwIHv338wtHd0gmliAScnB0NTY/3wCABYIBQX5ROtvrdv4vDJAqDYHNFZAB4LTEwMoqKiDIyMjDjVgPruL1++HJ4BAAIgz5eWVeKU7+5qH74p4N+/f3A2tvKA2vl+UKYAenh20AcArZL5kAmAEVsGIIPRMmC0DBgtA0bLgNEyYCRmAVBbH18yp9Us9qAJAFp0dOjeHY5f95Dh6ONvYHqogNExwdEAGOFgUFWD+Gakcc3uDtoAILYgFKODJ+keAGmmokSr3XBiGKaAWadfE612WKYAEFgYJE9cB+jEMC0DyAEDkQKGZTX4/fd3MB6RAQDy+JRjk8GYmEBgGo6ef/rxKRgTEwhULQNgBZ+1rDzRekDzg6SMAeCaT0T2PAzAAqHUvmzwFoLUmOrG5nkYsFdyGN5ZAJ/nowyjGcxkzYZvAFDq+SEdANTwPFEBMBhXiRLr+UG/VBbkkYP3DtLU84N2qSzMI+uvrGNYdn4p1ZP9oF4qi+6RU49PEQwEcvI8MW5noXfHhJ+fn+Hgo4MYHgEFAgiE6YYzvHv3Dr5qhJOTk4GNiw2v53VFdMl2L91rgW/fvjG4q7ljjTFQIKy6vJJBSEgIvHCKWM9//PiRKr3BAxcuXqJ5APz+/Rscw5EGUXgDQVhYmCaeh/rxALYsEAjaTQXaVUWPlCAuJsYQFRmOkvzRs8PTT09xep7zIzfDxOVTybEa5PlAGIcRNueGb40erUHGqvT5QCqBSOWJM8JmLqCGvSC/D4qWINBDiUBqAT09P+iawkQEAtU9P+j6AngCgSaeH5SdISyBQDPPwwuCwbiFHlQwAnECLe0A+RsgwADw6Ls5davQBwAAAABJRU5ErkJggg==' ' class="itemThumbnail">
                       </a>
                    </div>
        
                    <div class="item_right" style="float: none; width: auto; overflow: hidden;">
                        <a href='https://dev04875.esri.com/arcgis/home/group.html?id=6248d6d74ebf481aa2fe293654894f5a' target='_blank'><b>Central Services</b>
                        </a>
                        <br>
                        <br><b>Summary</b>: The authoritative service catalog.   
                        <br><b>Description</b>: This Group contains an inventory of map services for our organization. These map services serve as building blocks for all maps and apps throughout the organization. 
                        <br><b>Owner</b>: portaladmin
                        <br><b>Created</b>: June 04, 2016
                        
                    </div>
                </div>
                



<div class="9item_container" style="height: auto; overflow: hidden; border: 1px solid #cfcfcf; border-radius: 2px; background: #f6fafa; line-height: 1.21429em; padding: 10px;">
                    <div class="item_left" style="width: 210px; float: left;">
                       <a href='https://dev04875.esri.com/arcgis/home/group.html?id=3b2975a697d647ec844e22cfaa879c03' target='_blank'>
                        <img src='data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAEAAAABACAYAAACqaXHeAAAAAXNSR0IArs4c6QAAAARnQU1BAACxjwv8YQUAAAAJcEhZcwAADsMAAA7DAcdvqGQAAAAadEVYdFNvZnR3YXJlAFBhaW50Lk5FVCB2My41LjEwMPRyoQAAAidJREFUeF7tWj1LBDEQ3R9of3/C3lYOWzvB3sIP0N5aBDtBsPA4UI5j8QOx3i7yFiLZmK89M2eyMwvh7nazuczLezOT3WmUUg3n1lAdtYBKZX81rBIAqBBgJYFajHXNMxsB9ucHquTmW6SsALTnR6rEhoURADz5jjAgFwKgWYn0x5y2JgH2TrDrOhVrZxeX0T6xMcZe3xoDUibGAoDZ6ULZTYNjAuDql/Oc/k9hQCgPyJHGIpIA5SolQAFAVRKgACDEhOKcoACQ4Zmg7QNEAoGESCRQWiZI4QNEAiIB/6aIhQ8QCYgERAL1bIcpwqDsBbj7AIkC3BnA3geIBEQCkghJIuRzhCx2gxIFano1JnuBEY/F26dDZ61NLa/G7GIQLP6ois7F7U4SAKUmQn8C4GN1pQAAPm3ZsGDAy/1uDwA+WQIA43VzOc6S64N0gcTGEtD01wC4ZEBahpqrlM0aJ9kJrh72flYfIOB3Svgkmne2YQcAvC6PB0aalB/zHeNocLLNlGigXwz4ertRy7vZRkDgPtxvMoNo3tmG9UrApnyMAT5JZJsp0UBBH/D+fJLEBPQjr8T+DwBgVAyEkPE9uoUf0SiwfpwHWYDroWhQuP39NiC4F4hp37c3qDYKmKv52V4PVh8pMM7plFiDg3OT9AEm/c3YDmPNnCEkg6olgLjuiu16tc2cYXIMgHGp6S762QnQJHxASq4f61O1BGLGpVwXAApHAIv4DROIdJ5LvI5+AAAAAElFTkSuQmCC' ' class="itemThumbnail">
                       </a>
                    </div>
        
                    <div class="item_right" style="float: none; width: auto; overflow: hidden;">
                        <a href='https://dev04875.esri.com/arcgis/home/group.html?id=3b2975a697d647ec844e22cfaa879c03' target='_blank'><b>Compliance</b>
                        </a>
                        <br>
                        <br><b>Summary</b>: Regulatory compliance tracking & reporting.
                        <br><b>Description</b>: A group dealing with government and industry association regulatory compliance and reporting.
                        <br><b>Owner</b>: portaladmin
                        <br><b>Created</b>: June 04, 2016
                        
                    </div>
                </div>
                



<div class="9item_container" style="height: auto; overflow: hidden; border: 1px solid #cfcfcf; border-radius: 2px; background: #f6fafa; line-height: 1.21429em; padding: 10px;">
                    <div class="item_left" style="width: 210px; float: left;">
                       <a href='https://dev04875.esri.com/arcgis/home/group.html?id=3aa3bd16245a4969b6723f2fd19c2d5b' target='_blank'>
                        <img src='data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAEAAAABACAYAAACqaXHeAAAAGXRFWHRTb2Z0d2FyZQBBZG9iZSBJbWFnZVJlYWR5ccllPAAAAyRpVFh0WE1MOmNvbS5hZG9iZS54bXAAAAAAADw/eHBhY2tldCBiZWdpbj0i77u/IiBpZD0iVzVNME1wQ2VoaUh6cmVTek5UY3prYzlkIj8+IDx4OnhtcG1ldGEgeG1sbnM6eD0iYWRvYmU6bnM6bWV0YS8iIHg6eG1wdGs9IkFkb2JlIFhNUCBDb3JlIDUuMy1jMDExIDY2LjE0NTY2MSwgMjAxMi8wMi8wNi0xNDo1NjoyNyAgICAgICAgIj4gPHJkZjpSREYgeG1sbnM6cmRmPSJodHRwOi8vd3d3LnczLm9yZy8xOTk5LzAyLzIyLXJkZi1zeW50YXgtbnMjIj4gPHJkZjpEZXNjcmlwdGlvbiByZGY6YWJvdXQ9IiIgeG1sbnM6eG1wPSJodHRwOi8vbnMuYWRvYmUuY29tL3hhcC8xLjAvIiB4bWxuczp4bXBNTT0iaHR0cDovL25zLmFkb2JlLmNvbS94YXAvMS4wL21tLyIgeG1sbnM6c3RSZWY9Imh0dHA6Ly9ucy5hZG9iZS5jb20veGFwLzEuMC9zVHlwZS9SZXNvdXJjZVJlZiMiIHhtcDpDcmVhdG9yVG9vbD0iQWRvYmUgUGhvdG9zaG9wIENTNiAoTWFjaW50b3NoKSIgeG1wTU06SW5zdGFuY2VJRD0ieG1wLmlpZDo1MjVCNTUwNjc5NzcxMUUyODZGRkEwQ0EzMUFFRUU5NCIgeG1wTU06RG9jdW1lbnRJRD0ieG1wLmRpZDo1MjVCNTUwNzc5NzcxMUUyODZGRkEwQ0EzMUFFRUU5NCI+IDx4bXBNTTpEZXJpdmVkRnJvbSBzdFJlZjppbnN0YW5jZUlEPSJ4bXAuaWlkOjUyNUI1NTA0Nzk3NzExRTI4NkZGQTBDQTMxQUVFRTk0IiBzdFJlZjpkb2N1bWVudElEPSJ4bXAuZGlkOjUyNUI1NTA1Nzk3NzExRTI4NkZGQTBDQTMxQUVFRTk0Ii8+IDwvcmRmOkRlc2NyaXB0aW9uPiA8L3JkZjpSREY+IDwveDp4bXBtZXRhPiA8P3hwYWNrZXQgZW5kPSJyIj8+waTGmQAABRFJREFUeNrsW1FMW1UY/m8trBSVAgsZCWBjImVxD3RbsmHNxjQ8LHOy+uAsizrY5tPMNDHKg3FgjBo14cE9oOLYi8MHFYYPRBfXbgk6E+3tNpfBlq0VloGkjiK0VNJR///akpty20vvbtt72/snJ216zrmn/3e+/z/fOT1lotEoFLLpGYZJu1PXu+/lDWp6qR1fOXJY9c5//kUf6KDATS8HimozPnv1cjyw8803VOP8hx99cv8MaHPsh4pyk+qclyUEDpxoOzg8NgTsD+yqOusjVrCaN4PtMVv+AYCO78OXHixm4zojdOw8BOsfXL9S71/ww/jUGJw8/yUMu8/AC9sdHCB5AQA6T46/hmUIi722oo4dvT4Kk39PQGgpBMZiI9RW1nEOd9lb4AwCcOLsp9CyqQWe3+YA7wIDy1GVAsBzvj32kXN85hrYHm8Cc5VjpZ1vxgfsLfb/2W9ycKFAbCDbu2U/eKbDOXW0qqw0fQBitI87b9pQvqHnue12wNdVbc1VZq5Mz07DdxcHodmyiwsTAsFS3QChZQv8eCvItd1XXwpD14OCY2ai7slaAwIg7KOYEOqJ0T5Aznc83S7oPN+ontq5xp1caFBYfH1xAL9ECZToGcWFgC5VtqeJxdKNpZ9m3lBsWNNDqR21p9lv3dwK/nk/uL0/w9Zqg6pyQCuWU1garY82msRmXogJlpp6mLg7CbZ6G7A+N7y4owkqS3QrlE1mmaiTAkAzxT2Wgw01GyWhS/3iiZDCoWydjktGM3PBpEkpU3VphQDS3xxznrPyUpMkAPj9aLlUogkC8NXR075EOksxqf0UJ4RojaclLl2jfok2FwyJ0jITdVIA8FACpDezCwEEIH10qV/cSCmSGiwrNYrGq1QdICUHJAUAw8AaF0MeLzuIK0HaAGA/ONryKqcFyPk7iyrSATwghrx/+XxCdBaj/3xogXN+IsjAzXkGFiPq3Q3aT18YYNeiBMlIDmN76NzTCTPBCEzeXUqLllLXerlzAJ8FHgyF9pM/9ffbUeFtrGlI2vba7TEYxL1AW9MBqHi4BqbDD2BsFmVlrZc1BwiAcApB8AxcGOhHFqA6tEI1jw1TOOu0GyzSFXMzX2Wqwx3gIlQ8ZMyfEyFiAh38XPJeirJ/umH06i8rdXWVtfDSEy/jzs8CAWT87RADkWXl/3wg6UyQnKQiZL9NLcK/kago/dSgAyTZ1uqSlPVECn8oAn2ef2A2fC/t9VyqRhAFAOPbFDv8OMbfBwjZob4OyQDRpoiOyl7f9iz0ugNwZz6imBBwYnZv3L1lN5gkbn7WYoFgAEZ+HwH3993w1t7j0PPrrCATsgoAzn4XOe/Y4cj4gAQujYOrCZy9PAyHG/fAjQSdkAmNIMaAYzTz2TQar3fkM3jG2gpz94pydx4Qi31TJmmfjAl0RqDL8fZA+3UYxU0AWbCq4twf5+GpTTtlGyjV8xIpqmodIMWSxa4WAkqVwnLbt70fFzYAubxvpIWAEr5ELu8Z5RwAg8HASSG6e5gqFAikrnfeZrQQ0ADQANAAyKskGA6Ho/HLi2KrQapL2lITZF4IoftZRjUdoBQdoCVBDQANgMIEIHYhK+cAuJxXXFkdODaeK/FCVq5WAbvzinMQSzO/Us5DUdflc1zhf0TjKkIJ0skwvuxKoKbsv23jOIyWAzQANAA0AFQDQHhJnr+5yPWcbG+Gut//5oPjMo7VrUQAmEL/+/x/AgwADZ0ipIwo65cAAAAASUVORK5CYII=' ' class="itemThumbnail">
                       </a>
                    </div>
        
                    <div class="item_right" style="float: none; width: auto; overflow: hidden;">
                        <a href='https://dev04875.esri.com/arcgis/home/group.html?id=3aa3bd16245a4969b6723f2fd19c2d5b' target='_blank'><b>Customer Service, Finance, Billing and Accounting</b>
                        </a>
                        <br>
                        <br><b>Summary</b>: The Water & Sewer Billing and Collection Division manage the water and sewer accounts for residents.
                        <br><b>Description</b>: Typical types of users and roles that part of this group:Human Resources/PayrollAdministrative Services Officer I, IIAdministrative AideAccountantAccount ClerkPayroll ClerkDepartment AnalystRisk Management Analyst I,II,IIIAccounting TechnicianEngineering TechnicianCommon Task or Responsibilities of the users in this group:Customer ServiceReal Estate ServicesBudgets/Accounting/Long Range Financial PlanningSafety/Risk ManagementFinancingRecords Management/Clerical ServicesAuditingBill Collection and paymentConnections/DisconnectsReduced Rates / Leak AdjustmentsPool AdjustmentIrrigation and Hydrant MetersEmployee Development
                        <br><b>Owner</b>: portaladmin
                        <br><b>Created</b>: June 04, 2016
                        
                    </div>
                </div>
                



<div class="9item_container" style="height: auto; overflow: hidden; border: 1px solid #cfcfcf; border-radius: 2px; background: #f6fafa; line-height: 1.21429em; padding: 10px;">
                    <div class="item_left" style="width: 210px; float: left;">
                       <a href='https://dev04875.esri.com/arcgis/home/group.html?id=c9dc1e4757ba40849ffdb69fc3842f87' target='_blank'>
                        <img src='data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAEEAAABBCAYAAACO98lFAAAAGXRFWHRTb2Z0d2FyZQBBZG9iZSBJbWFnZVJlYWR5ccllPAAAAyRpVFh0WE1MOmNvbS5hZG9iZS54bXAAAAAAADw/eHBhY2tldCBiZWdpbj0i77u/IiBpZD0iVzVNME1wQ2VoaUh6cmVTek5UY3prYzlkIj8+IDx4OnhtcG1ldGEgeG1sbnM6eD0iYWRvYmU6bnM6bWV0YS8iIHg6eG1wdGs9IkFkb2JlIFhNUCBDb3JlIDUuMy1jMDExIDY2LjE0NTY2MSwgMjAxMi8wMi8wNi0xNDo1NjoyNyAgICAgICAgIj4gPHJkZjpSREYgeG1sbnM6cmRmPSJodHRwOi8vd3d3LnczLm9yZy8xOTk5LzAyLzIyLXJkZi1zeW50YXgtbnMjIj4gPHJkZjpEZXNjcmlwdGlvbiByZGY6YWJvdXQ9IiIgeG1sbnM6eG1wPSJodHRwOi8vbnMuYWRvYmUuY29tL3hhcC8xLjAvIiB4bWxuczp4bXBNTT0iaHR0cDovL25zLmFkb2JlLmNvbS94YXAvMS4wL21tLyIgeG1sbnM6c3RSZWY9Imh0dHA6Ly9ucy5hZG9iZS5jb20veGFwLzEuMC9zVHlwZS9SZXNvdXJjZVJlZiMiIHhtcDpDcmVhdG9yVG9vbD0iQWRvYmUgUGhvdG9zaG9wIENTNiAoTWFjaW50b3NoKSIgeG1wTU06SW5zdGFuY2VJRD0ieG1wLmlpZDo4QjIzODhFOENGRkIxMUUzQTFEREJDMEIzNzNGMjc5RCIgeG1wTU06RG9jdW1lbnRJRD0ieG1wLmRpZDo4QjIzODhFOUNGRkIxMUUzQTFEREJDMEIzNzNGMjc5RCI+IDx4bXBNTTpEZXJpdmVkRnJvbSBzdFJlZjppbnN0YW5jZUlEPSJ4bXAuaWlkOjhCMjM4OEU2Q0ZGQjExRTNBMUREQkMwQjM3M0YyNzlEIiBzdFJlZjpkb2N1bWVudElEPSJ4bXAuZGlkOjhCMjM4OEU3Q0ZGQjExRTNBMUREQkMwQjM3M0YyNzlEIi8+IDwvcmRmOkRlc2NyaXB0aW9uPiA8L3JkZjpSREY+IDwveDp4bXBtZXRhPiA8P3hwYWNrZXQgZW5kPSJyIj8+/VquXQAABPBJREFUeNrsm+9vU1UYx5/Clo1f+8FYWTWjLcKyQPYDIhgQTGeiUXhB9YWR7U0X3vhuIzHEN4a+g/jG8Qcsq4ktyRIjJGiIRHcFCYbE2Q6Ny0RuL4tpJ8yttINtvTCf53o7usLa23tvz9pxvslNe+957jk9n3uec76n3SwLCwvwomsNcHEIHAKH8FRlRm7uOtfpxRcPHva0yxIePn9PwGv2h/3msw+Wbe/oqUHd7Vn0rA7Y+Rp8EZobm9s6WlzQUNuwWBadisLQLQFGx0dDeOpCGNMmdF5pr76+rm270wEbN25YLEskZuCOGIZ79yaV9hDGNKt0EDpaXW3H3/hwCQASndN1Kqc4kwaBsN1pb2tt2b0EAInO6TqV621vjZ4UoBHgwhGQTVROcWrKGEoBGgFOpz1rHJVTnJoyBR8Jno4cAFJS4zwGR4GHUkCL1DgPCwj2zBRYTmqc3SAEe2YKLCc1zs5qTsg3hRzAUJgSjqKDgCtEmCUEXCHChYYg0TKoRWqcZLBPEi2DWqTGSSzSwUc+QIvUOJ9BCD7yAVqkxuXdnl6zFCQfkG2ZFBDA0IgQwlRoNyHHg+QDsi2ToighBCmEqZB3e3ptsws7KESmojkdo0lp7sIOCvFEIqdjZGabjewdjvuG6YP24nEso+giHn3nPXuFktg76BUC6KteV95zeMcWaNq6CSrK/p+S5uQnMDYRh2u370PsUfILBOEBhsoJAZ+2m56QUdOzUPkmWOsPQtf+bYudzxTB8N+8C1UTP8dfi1/eZEL/KEV6cZQIuiEQAMz3r9999R1wWPX7HXHyMXwVlOHE685lAaSD6L8uwkcNQWjeEDNEIBKZgLE//wJZlvcgiKDeJbLvvQNuQwBIN8QkUArkAkCiGIr9btJmeBjYbFuhaecr9NZrxCdo3idk0x9RWZkDtIpif32w2ZR8JxDPmYTZ2+bUE84ntrJ8Ldyfr2Dy2fKCcNrvZcUMZpOPIfTTFV33fv/D1eLbRaYmvELEMh8Jutfh5G3FB2gVxe6p+nd1QYD5XxQjpOUJUwzFvl0XWWUjQR6HXbjIkBHKBiJllvavv2vYIxTlnHBkdwW8VCUrRujW37ElMOg9XaOy1nIROm0i0zmhjGVj77dXKu5x4Oo1uFS+I7P44lvTgWOdhxzAWmWsG3TWrQXLwwsQ6AlYnt0lnkUPv0ogRB48gaGxecUpKqr+GD69lHgagOe4o3x202L9BL78Lf38oHJOK4XbOg7bKmdKAwIB8N+cg33OLeBurzXFX4zgfPH5nc1wsjFUEBCmT4zf/j4Hh3daYZ+91pT6yEJTXYeabBCIOEtjdQjjxNfycrXpH5TqlOZrVwbCbHJWc2XTD9naXS2SZdkwhNjAFZ9mEDXrC2s7aFOVL4Dh4RF6+6ORidEVnYoKZwbP6h7f/TfG4cSBRub3pu0k6YvYXt0Q/D0B+kqqJnXeda6zZP7e7+ipQUvR2eZiFofAIXAIum1z92m/dyBrBO4LCimN3x92FwwCrhY+yPHT93M3Ris06/N04BA4BA6BQygmCP/E5wpWJ64+jlKAINFvB2aCoLqoTqr7vGdveKXNkha5cd9/of+6aDfc+dgMnLk8mr4ldheDY8wpfFK0/XaYZZ6wPktJzgl8YuQQOIQXZiutR904ww/ovZcFBAv/X2meDhwCh8AhLNV/AgwAxr0HBzcL588AAAAASUVORK5CYII=' ' class="itemThumbnail">
                       </a>
                    </div>
        
                    <div class="item_right" style="float: none; width: auto; overflow: hidden;">
                        <a href='https://dev04875.esri.com/arcgis/home/group.html?id=c9dc1e4757ba40849ffdb69fc3842f87' target='_blank'><b>Demographic Content</b>
                        </a>
                        <br>
                        <br><b>Summary</b>: Esri demographic data with national coverage
                        <br><b>Description</b>: A catalog of Esri provided demographic content for use at the utility.
                        <br><b>Owner</b>: portaladmin
                        <br><b>Created</b>: June 04, 2016
                        
                    </div>
                </div>
                



<div class="9item_container" style="height: auto; overflow: hidden; border: 1px solid #cfcfcf; border-radius: 2px; background: #f6fafa; line-height: 1.21429em; padding: 10px;">
                    <div class="item_left" style="width: 210px; float: left;">
                       <a href='https://dev04875.esri.com/arcgis/home/group.html?id=e125d31fd14941b3851d4efc3ad612a3' target='_blank'>
                        <img src='data:image/png;base64,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' ' class="itemThumbnail">
                       </a>
                    </div>
        
                    <div class="item_right" style="float: none; width: auto; overflow: hidden;">
                        <a href='https://dev04875.esri.com/arcgis/home/group.html?id=e125d31fd14941b3851d4efc3ad612a3' target='_blank'><b>Design and Engineering</b>
                        </a>
                        <br>
                        <br><b>Summary</b>: Design and Engineering is responsible for designing, planning and inspection construction and maintenance projects.
                        <br><b>Description</b>: Typical types of users and roles that part of this group:Chief EngineerAsst. Chief EngineerEngineeringEngineering TechInspectorElectricianLand SurveyorTechnical WriterCommon Task or Responsibilities of the users in this group:DesignConstructionEngineering/Survey ServicesPlanning
                        <br><b>Owner</b>: portaladmin
                        <br><b>Created</b>: June 04, 2016
                        
                    </div>
                </div>
                



<div class="9item_container" style="height: auto; overflow: hidden; border: 1px solid #cfcfcf; border-radius: 2px; background: #f6fafa; line-height: 1.21429em; padding: 10px;">
                    <div class="item_left" style="width: 210px; float: left;">
                       <a href='https://dev04875.esri.com/arcgis/home/group.html?id=95e389636a9d4a74bd74387eefa5fe79' target='_blank'>
                        <img src='data:image/png;base64,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' ' class="itemThumbnail">
                       </a>
                    </div>
        
                    <div class="item_right" style="float: none; width: auto; overflow: hidden;">
                        <a href='https://dev04875.esri.com/arcgis/home/group.html?id=95e389636a9d4a74bd74387eefa5fe79' target='_blank'><b>Disaster Response</b>
                        </a>
                        <br>
                        <br><b>Summary</b>: A catalog of Esri Live Feeds which may be needed during a disaster
                        <br><b>Description</b>: A catalog of Esri Live Feeds which may be needed during a disaster.During disasters, the Esri Disaster Response Program receives numerous requests for access to live disaster data. In the United States and abroad, many data sources are already freely available from agencies, but not in a readily digestible or dynamic format* for Web GIS platforms. To unlock the potential of these data sets, we use the Aggregated Live Feed methodology.
                        <br><b>Owner</b>: portaladmin
                        <br><b>Created</b>: June 04, 2016
                        
                    </div>
                </div>
                



<div class="9item_container" style="height: auto; overflow: hidden; border: 1px solid #cfcfcf; border-radius: 2px; background: #f6fafa; line-height: 1.21429em; padding: 10px;">
                    <div class="item_left" style="width: 210px; float: left;">
                       <a href='https://dev04875.esri.com/arcgis/home/group.html?id=4954ded8179c4398a2a17b800bd049c4' target='_blank'>
                        <img src='data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAEAAAABACAYAAACqaXHeAAAAGXRFWHRTb2Z0d2FyZQBBZG9iZSBJbWFnZVJlYWR5ccllPAAAAyRpVFh0WE1MOmNvbS5hZG9iZS54bXAAAAAAADw/eHBhY2tldCBiZWdpbj0i77u/IiBpZD0iVzVNME1wQ2VoaUh6cmVTek5UY3prYzlkIj8+IDx4OnhtcG1ldGEgeG1sbnM6eD0iYWRvYmU6bnM6bWV0YS8iIHg6eG1wdGs9IkFkb2JlIFhNUCBDb3JlIDUuMy1jMDExIDY2LjE0NTY2MSwgMjAxMi8wMi8wNi0xNDo1NjoyNyAgICAgICAgIj4gPHJkZjpSREYgeG1sbnM6cmRmPSJodHRwOi8vd3d3LnczLm9yZy8xOTk5LzAyLzIyLXJkZi1zeW50YXgtbnMjIj4gPHJkZjpEZXNjcmlwdGlvbiByZGY6YWJvdXQ9IiIgeG1sbnM6eG1wPSJodHRwOi8vbnMuYWRvYmUuY29tL3hhcC8xLjAvIiB4bWxuczp4bXBNTT0iaHR0cDovL25zLmFkb2JlLmNvbS94YXAvMS4wL21tLyIgeG1sbnM6c3RSZWY9Imh0dHA6Ly9ucy5hZG9iZS5jb20veGFwLzEuMC9zVHlwZS9SZXNvdXJjZVJlZiMiIHhtcDpDcmVhdG9yVG9vbD0iQWRvYmUgUGhvdG9zaG9wIENTNiAoTWFjaW50b3NoKSIgeG1wTU06SW5zdGFuY2VJRD0ieG1wLmlpZDo1MjVCNTUwQTc5NzcxMUUyODZGRkEwQ0EzMUFFRUU5NCIgeG1wTU06RG9jdW1lbnRJRD0ieG1wLmRpZDo1MzA0MzYyMDc5NzcxMUUyODZGRkEwQ0EzMUFFRUU5NCI+IDx4bXBNTTpEZXJpdmVkRnJvbSBzdFJlZjppbnN0YW5jZUlEPSJ4bXAuaWlkOjUyNUI1NTA4Nzk3NzExRTI4NkZGQTBDQTMxQUVFRTk0IiBzdFJlZjpkb2N1bWVudElEPSJ4bXAuZGlkOjUyNUI1NTA5Nzk3NzExRTI4NkZGQTBDQTMxQUVFRTk0Ii8+IDwvcmRmOkRlc2NyaXB0aW9uPiA8L3JkZjpSREY+IDwveDp4bXBtZXRhPiA8P3hwYWNrZXQgZW5kPSJyIj8+tQigmgAABLBJREFUeNrsW01ME1EQnpZVCFZYMDQh0YiIQeylepCDmEC0wg3QxKsaExMPIh40mmAUJfEv4c+bxoAx8WCiWOUAVkMTkQTjD4mWqtGIlESkakr9rdbUmbXVUtv9Kbvttuwkj9e3+2bfm+/NvJl5u+gCgQDMZdKpeXJ7GvYpujqdHa06Ru0rtKvppCLPPddykKv1MMeJSZWJttwek+U5TRuLUhOAyInLRZoJqHlyZ06fwL+B9AJAimvbf+BQnKApCAAKwGK1HUstFjMWanuwjGCxYuk+29nmSaZr23btTcw+FzcvjR8AFJ4EbzOZVrEmUxksWbw4dIt1TUxUOhxOLKNHsN8+BKE7WaYTKaQsGoBCNRoLCtqqayyA9X/3CQwqa9aY2f4+WxdpCoLQngzXJrsJoDB1JPzWrVsgMzOTty+BQ/2uXLnahnxjCML1RLu2aCYQSysYkTbfRSsvJHyIqB/1v3TpMmmCXWhPULsJ1JHNR1N7IU0gPtwT6mhjDL+3Ikc9GagYAGpLlhfH9XDiQwBqIwFQmuT2AmxObk5cEwnysfFMNHyyYvsp5gWkqr9YPrETlSKQIl4AfXy4zxdNxJcMitQYPgDFADDmnfYCSJcfOD7kl2oCkRNOtglYHaPO7bijSwYA+SAYHqeuCVAgQwENqnORFDMg9Xe5JngDoVTxAkT1VmvvY4rwxGyIU243YH+OL96JqsoL4CqOoBbswPC2q6baAiUly2P2ffnyFfT128Dn8+0gvrTwAkEQuhGEEeuN3i7UAjNlgwXGf9rgnnJj0OOk1SehBYVPhglEA1JSOhwUajUCUTlld4fOA0LEnQdgH7sctirWBKIJpUg6HAEECWmf7UqllAmEp8ZY9fBtmHw7/7HmI0kxAcF0OJj2NmLZyxe/E9FGGC0uwMSHNsAefFZM3hMnT8H6igqwWDbA4TtvYXz6h+CqJ8oLDOTn55uXFRfz5v33h4fB6/0UPfLD6wzDwNry8pj86B3gwcNH8NThgOONDeowAVyxoyT8yrIyQYaQcJ8/fwGDYcHf69QGfQav8KHDEhrnmdMJNtsdThPkJimnySEN2EsrL4UGh4agZpNlRjs3N080P413d3AQ1lVWwe6bLtlygaysLNFvvDs7WoEJ2j4r9rgrRJOT78Dh0YGJDXA1taUAQON9+/YdsufpBVVcSROY1auxgUk9eH/+qVOVGDqwjLZrP3/+DEpLV/Iy+34BnH+Rwf02CAzE9zwhtyUlslM8DlCCUtYE0oFUoQHxvARNGwCkuC3NBNLVBOJIyES/WkIvp9M0IN00QMzqitUSTQNUaN+Um7wWOpMQu8o8feiVfZ5aTYBtatyp6AAt7RdYVZpAoj+mUO0mODV6WwuENAA0ADQANADSHgAMVIrUAIDd5XIldODgePQR5ZgaQuF61/h4D5bKGXdLeZDTZwDz9SP4s/O5mtp89OH9Bxh6fy/8kh0EPqBIGADB6KtKSpxdWFgI/lcDMF1aDQuxpvZsc/OU2gMMBgMsWjAf8p5c5Wpqz7ls0Gg0ckXzAhoAaQqA3++XZRC5npPoPaD5/vCwnN+zNEvo6zl36w2rsOzcuYNurv/7/G8BBgAYEtxWuu0zcwAAAABJRU5ErkJggg==' ' class="itemThumbnail">
                       </a>
                    </div>
        
                    <div class="item_right" style="float: none; width: auto; overflow: hidden;">
                        <a href='https://dev04875.esri.com/arcgis/home/group.html?id=4954ded8179c4398a2a17b800bd049c4' target='_blank'><b>Executive Reports</b>
                        </a>
                        <br>
                        <br><b>Summary</b>: Maps, apps and reports for the Board or Executives.
                        <br><b>Description</b>: This group is used to catalog key performance indicators and content used by the Board or Executive of our organization.  
                        <br><b>Owner</b>: portaladmin
                        <br><b>Created</b>: June 04, 2016
                        
                    </div>
                </div>
                



<div class="9item_container" style="height: auto; overflow: hidden; border: 1px solid #cfcfcf; border-radius: 2px; background: #f6fafa; line-height: 1.21429em; padding: 10px;">
                    <div class="item_left" style="width: 210px; float: left;">
                       <a href='https://dev04875.esri.com/arcgis/home/group.html?id=6245ec58f0284eb38befd5aa2807fcc4' target='_blank'>
                        <img src='data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAEAAAABACAYAAACqaXHeAAAAGXRFWHRTb2Z0d2FyZQBBZG9iZSBJbWFnZVJlYWR5ccllPAAAAyRpVFh0WE1MOmNvbS5hZG9iZS54bXAAAAAAADw/eHBhY2tldCBiZWdpbj0i77u/IiBpZD0iVzVNME1wQ2VoaUh6cmVTek5UY3prYzlkIj8+IDx4OnhtcG1ldGEgeG1sbnM6eD0iYWRvYmU6bnM6bWV0YS8iIHg6eG1wdGs9IkFkb2JlIFhNUCBDb3JlIDUuMy1jMDExIDY2LjE0NTY2MSwgMjAxMi8wMi8wNi0xNDo1NjoyNyAgICAgICAgIj4gPHJkZjpSREYgeG1sbnM6cmRmPSJodHRwOi8vd3d3LnczLm9yZy8xOTk5LzAyLzIyLXJkZi1zeW50YXgtbnMjIj4gPHJkZjpEZXNjcmlwdGlvbiByZGY6YWJvdXQ9IiIgeG1sbnM6eG1wPSJodHRwOi8vbnMuYWRvYmUuY29tL3hhcC8xLjAvIiB4bWxuczp4bXBNTT0iaHR0cDovL25zLmFkb2JlLmNvbS94YXAvMS4wL21tLyIgeG1sbnM6c3RSZWY9Imh0dHA6Ly9ucy5hZG9iZS5jb20veGFwLzEuMC9zVHlwZS9SZXNvdXJjZVJlZiMiIHhtcDpDcmVhdG9yVG9vbD0iQWRvYmUgUGhvdG9zaG9wIENTNiAoTWFjaW50b3NoKSIgeG1wTU06SW5zdGFuY2VJRD0ieG1wLmlpZDo1NTA4MDNBMTc5NzcxMUUyODZGRkEwQ0EzMUFFRUU5NCIgeG1wTU06RG9jdW1lbnRJRD0ieG1wLmRpZDo1NTA4MDNBMjc5NzcxMUUyODZGRkEwQ0EzMUFFRUU5NCI+IDx4bXBNTTpEZXJpdmVkRnJvbSBzdFJlZjppbnN0YW5jZUlEPSJ4bXAuaWlkOjU1MDgwMzlGNzk3NzExRTI4NkZGQTBDQTMxQUVFRTk0IiBzdFJlZjpkb2N1bWVudElEPSJ4bXAuZGlkOjU1MDgwM0EwNzk3NzExRTI4NkZGQTBDQTMxQUVFRTk0Ii8+IDwvcmRmOkRlc2NyaXB0aW9uPiA8L3JkZjpSREY+IDwveDp4bXBtZXRhPiA8P3hwYWNrZXQgZW5kPSJyIj8+5wrENgAAA39JREFUeNrsm89PE0EUx7/b3+1ut11oKQX6K62akuAFg4qQEA/oEbyYeFH/Bm9e+hd48C8QLxovnjl4MPGoiREjBmI0tDRRQEBoa2t/ObPQChZCgd12y85Lpjv7o7vzPvPmvXnTLVetVqFnMUDnwgAwAAwAA8AA6FlMtQrHcQ0nbz9buEc2T1R69v0Xdy7MtEvx2vzHdMR1iUc3++FxmBR9+FquhAez6QSpzmh2CNDeHwsJYaWVp0LvSe+9a2Ga9QGJ6bhLtQfv3juhGR9AeqOeFDjMBtyIiVCj9/dawXTcHeaeL1bblI9wDT7g6a1QS1swRaxgSkUrO0zuvlxiYZABYAAYAAaAAfg/DO4ND7oE0ONy6ELplV85NgQYAAaAAWAAGAAGgAGAqRMbbSWt9joq8PJVUifFSPb5SsN1hRKH1dzOavdq1oCtAtnPcp0HgCo6IFbqhSrdrES7Go89nO0AAFTpQW+ZKFCGaK3qYwhQ045KZVwJlFRVWnMALEYOXssSrsd4dAu8vpxg3GvC5YCEL+n3MHIW/USBPqcFYyEr3Ha7vJ8rbECwxc4+ACNXwkjAjSHfv7WHQjFDPiswGXeas5ndlOuCTThbALodJkxEuuTtvnhdzMJJlKWKz6cXYDTYUSoXEfT4EOgeOBsAopIB4xFJdngNjTBasJ3PYC75GVH/KCQhIA+J+eQsbGYbvKKnswFQRzce7jr0vMMqwes6hwHPRdL7lvqxqH8MX1c+4GPyEyTeLQ8Jwc7D7+7tHAATERHnPbYjrwv1XGo4Ri2BFipbuR/EKtaRXk8htZbGSGxY+8lQs8o3I6LDh14pjsHgJKowI5PPaBuAksrvd5gZUrZl36BZAEM+myrKU1lem5MjQy1kag5AWLLialBUbVhRXyDxyr1ToKgTdJLEnJq+mkIjxVzyHYEgwk1A0IhwGmtQFMBkzHVgnFc0TXZF5bKRSWF9O4VvK2/JkOhDpCfUXgDD/XzDDE9NqYVI6hQX069hs3w/0fxAER9AFR/ua20aW19DMAtkwnSNWMLJfthVBMBo0NnWlJrOGmn+cJK5wakB0HDnd5rbvqgiCUGsbv1sPYB2mf5BM0WaTbYUAO19Gvq0IFYzj9/FfGsBaKX3a84w/+f4APbFrb1vTjQjj9/koC0Zx6vlyrG+wbH/DepcGAAGgAFgABgAPctfAQYA7DnTGMcYWhgAAAAASUVORK5CYII=' ' class="itemThumbnail">
                       </a>
                    </div>
        
                    <div class="item_right" style="float: none; width: auto; overflow: hidden;">
                        <a href='https://dev04875.esri.com/arcgis/home/group.html?id=6245ec58f0284eb38befd5aa2807fcc4' target='_blank'><b>External Contractors</b>
                        </a>
                        <br>
                        <br><b>Summary</b>: Collection of maps and apps designed to coordinate with external contractors and engineering firms.
                        <br><b>Description</b>: Typical types of users and roles that part of thisEngineering ContractorsContracted OperationsCommon Task or Responsibilities of the external users.  Valve ExercisingHydrant FlushingDesignRehabilitationCCTV      
                        <br><b>Owner</b>: portaladmin
                        <br><b>Created</b>: June 04, 2016
                        
                    </div>
                </div>
                



<div class="9item_container" style="height: auto; overflow: hidden; border: 1px solid #cfcfcf; border-radius: 2px; background: #f6fafa; line-height: 1.21429em; padding: 10px;">
                    <div class="item_left" style="width: 210px; float: left;">
                       <a href='https://dev04875.esri.com/arcgis/home/group.html?id=51512ccdb383491d9dbd8ca4040cd87e' target='_blank'>
                        <img src='data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAEAAAABACAYAAACqaXHeAAAAGXRFWHRTb2Z0d2FyZQBBZG9iZSBJbWFnZVJlYWR5ccllPAAAAyRpVFh0WE1MOmNvbS5hZG9iZS54bXAAAAAAADw/eHBhY2tldCBiZWdpbj0i77u/IiBpZD0iVzVNME1wQ2VoaUh6cmVTek5UY3prYzlkIj8+IDx4OnhtcG1ldGEgeG1sbnM6eD0iYWRvYmU6bnM6bWV0YS8iIHg6eG1wdGs9IkFkb2JlIFhNUCBDb3JlIDUuMy1jMDExIDY2LjE0NTY2MSwgMjAxMi8wMi8wNi0xNDo1NjoyNyAgICAgICAgIj4gPHJkZjpSREYgeG1sbnM6cmRmPSJodHRwOi8vd3d3LnczLm9yZy8xOTk5LzAyLzIyLXJkZi1zeW50YXgtbnMjIj4gPHJkZjpEZXNjcmlwdGlvbiByZGY6YWJvdXQ9IiIgeG1sbnM6eG1wPSJodHRwOi8vbnMuYWRvYmUuY29tL3hhcC8xLjAvIiB4bWxuczp4bXBNTT0iaHR0cDovL25zLmFkb2JlLmNvbS94YXAvMS4wL21tLyIgeG1sbnM6c3RSZWY9Imh0dHA6Ly9ucy5hZG9iZS5jb20veGFwLzEuMC9zVHlwZS9SZXNvdXJjZVJlZiMiIHhtcDpDcmVhdG9yVG9vbD0iQWRvYmUgUGhvdG9zaG9wIENTNiAoTWFjaW50b3NoKSIgeG1wTU06SW5zdGFuY2VJRD0ieG1wLmlpZDo1REFGN0I4Mjc5NzgxMUUyODZGRkEwQ0EzMUFFRUU5NCIgeG1wTU06RG9jdW1lbnRJRD0ieG1wLmRpZDo1REFGN0I4Mzc5NzgxMUUyODZGRkEwQ0EzMUFFRUU5NCI+IDx4bXBNTTpEZXJpdmVkRnJvbSBzdFJlZjppbnN0YW5jZUlEPSJ4bXAuaWlkOjVEQUY3QjgwNzk3ODExRTI4NkZGQTBDQTMxQUVFRTk0IiBzdFJlZjpkb2N1bWVudElEPSJ4bXAuZGlkOjVEQUY3QjgxNzk3ODExRTI4NkZGQTBDQTMxQUVFRTk0Ii8+IDwvcmRmOkRlc2NyaXB0aW9uPiA8L3JkZjpSREY+IDwveDp4bXBtZXRhPiA8P3hwYWNrZXQgZW5kPSJyIj8+G2jWjgAABD1JREFUeNrsW1to01AY/pq1y6h1c+su6ta5OZ1M5x68ghcYKjIYCMoUFYQJItP55gVBX3wQxcubiqgPPngBQR9EwQdFXxScA182lelct6rz0u7SuXRN28yczGm7tmm2Jlna5INwkp6/Sf7//N/5//MfYhodHYWeQUHnMI+f3D1RpytX2HX6iSnCAAQb913UhfLPbhw2KBBFgXD8fPdUsQfa569AHn/0fW6Fhz/UROHiTdIMQLBwU5NiL+JxXuYN0SwYQi18fHpVG1EgyP7Cb/dzodVlGGT6WyJa3Rlg2PM8otWVAYjbs4xTOCetFmhATYf7x7ue1L0YH9xuj9DKHgbDcfbcBUk3yqJpHGjaD5pv48E30BJ1nV1UH1c+FAqho6NDaMMRCAbR4/oGjuNAURRKHXNhMcdWpbq6OjkDEOxvPJRQxmQywUwF0e+6A++Px5KMNjLUju7Whph9xDC2ogZUVVVF9ZGRd3Z/Ec6JERwlDuTn26Pk2tra1KMAWVUOec3IntOIggXHQGXMmBon+f+R/+c69qLL2RtTxmq1wmKxCOekJdeKUSAcV84/kSS3ectSLFt9AZ6uS8IoS0XWzCWwlx+CObNgbKJk2X99L11MpIcsWgmOHQGVmYW3Hn4d5/nfv9ZhVcYAB4/WSZb1MRQKK0/B23sPA9/uJZSfNXcHcvgjHqKVssriuYpFgQDLYcDth61wu6BcMsqrvhhKlgLh2NO0DtZRp6jMeG4ghokUEIMmKDAeGXLygnC9FY/3DB8OudCw6MQ5GaU0kwhl0hSv3JvI36xlwkRH2kgjvEk/CmxvXI08uiUittO5W/HpgxeVS87A5771L2fw8VmhzV6bPhQg7p9bQPPu3y64tr28GbRtJYYGWZSUZmGE4YScgeZDn6frskADuZTSBAXG3L9FcPXimit8vF6OwT4/z/Wx2itpyTX5nfSTHCCREVKKAqvWV2BDXTlySk/i9vXX6O50x/Qo5ncADx4+wu6dp0RXhykZBUgyFAxwqN8mXvrq6XGNvczfDFBNCpiVdC+SDGkdiidCcnlUSlJATqRkIpQKmJQHXLt5SdaHSym0aIoCBMW1s0X7C3IqUDFnTcL73L96Pv0oYM7IREl+TfpSQAx5Mx2YV7gCtMWmyItqhgLxlK8srpUky/j74WW+ayYKmJN1+aVl9ZJG/ddgJ7p/tiIYYtOHArNzqyQr39n7akrPkFLanjYDZFuLEsqEOFYY+akg0abGtEcBxt8nafS15vayGcAfGE4o42V+aDoMJmUAKbO5UmFRtc3Rifj6Ilzp7+jAe8VGJ94EqPrm6FRyd7kQSwFNbo6qCc1vjqpRENH85qjyBRGNb46m5WrQqAkaNUGDAgYFDArokQJyl8NTqiBy/NiRaVNe8wWRdIBhAGMSjIN439jowgCxvq4yKKAHDwj/olIvMBlfj+scfwQYANL8vLsE+JikAAAAAElFTkSuQmCC' ' class="itemThumbnail">
                       </a>
                    </div>
        
                    <div class="item_right" style="float: none; width: auto; overflow: hidden;">
                        <a href='https://dev04875.esri.com/arcgis/home/group.html?id=51512ccdb383491d9dbd8ca4040cd87e' target='_blank'><b>Featured Apps</b>
                        </a>
                        <br>
                        <br><b>Summary</b>: Apps that appear on the organization home page.
                        <br><b>Description</b>: This group contains a collection of applications highlighted on the organization's home page. 
                        <br><b>Owner</b>: portaladmin
                        <br><b>Created</b>: June 04, 2016
                        
                    </div>
                </div>
                



<div class="9item_container" style="height: auto; overflow: hidden; border: 1px solid #cfcfcf; border-radius: 2px; background: #f6fafa; line-height: 1.21429em; padding: 10px;">
                    <div class="item_left" style="width: 210px; float: left;">
                       <a href='https://dev04875.esri.com/arcgis/home/group.html?id=e090429c526b4f549b23c59fddc1f12c' target='_blank'>
                        <img src='data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAEAAAABACAYAAACqaXHeAAAAGXRFWHRTb2Z0d2FyZQBBZG9iZSBJbWFnZVJlYWR5ccllPAAAAyRpVFh0WE1MOmNvbS5hZG9iZS54bXAAAAAAADw/eHBhY2tldCBiZWdpbj0i77u/IiBpZD0iVzVNME1wQ2VoaUh6cmVTek5UY3prYzlkIj8+IDx4OnhtcG1ldGEgeG1sbnM6eD0iYWRvYmU6bnM6bWV0YS8iIHg6eG1wdGs9IkFkb2JlIFhNUCBDb3JlIDUuMy1jMDExIDY2LjE0NTY2MSwgMjAxMi8wMi8wNi0xNDo1NjoyNyAgICAgICAgIj4gPHJkZjpSREYgeG1sbnM6cmRmPSJodHRwOi8vd3d3LnczLm9yZy8xOTk5LzAyLzIyLXJkZi1zeW50YXgtbnMjIj4gPHJkZjpEZXNjcmlwdGlvbiByZGY6YWJvdXQ9IiIgeG1sbnM6eG1wPSJodHRwOi8vbnMuYWRvYmUuY29tL3hhcC8xLjAvIiB4bWxuczp4bXBNTT0iaHR0cDovL25zLmFkb2JlLmNvbS94YXAvMS4wL21tLyIgeG1sbnM6c3RSZWY9Imh0dHA6Ly9ucy5hZG9iZS5jb20veGFwLzEuMC9zVHlwZS9SZXNvdXJjZVJlZiMiIHhtcDpDcmVhdG9yVG9vbD0iQWRvYmUgUGhvdG9zaG9wIENTNiAoTWFjaW50b3NoKSIgeG1wTU06SW5zdGFuY2VJRD0ieG1wLmlpZDo1MzA0MzYyNzc5NzcxMUUyODZGRkEwQ0EzMUFFRUU5NCIgeG1wTU06RG9jdW1lbnRJRD0ieG1wLmRpZDo1MzA0MzYyODc5NzcxMUUyODZGRkEwQ0EzMUFFRUU5NCI+IDx4bXBNTTpEZXJpdmVkRnJvbSBzdFJlZjppbnN0YW5jZUlEPSJ4bXAuaWlkOjUzMDQzNjI1Nzk3NzExRTI4NkZGQTBDQTMxQUVFRTk0IiBzdFJlZjpkb2N1bWVudElEPSJ4bXAuZGlkOjUzMDQzNjI2Nzk3NzExRTI4NkZGQTBDQTMxQUVFRTk0Ii8+IDwvcmRmOkRlc2NyaXB0aW9uPiA8L3JkZjpSREY+IDwveDp4bXBtZXRhPiA8P3hwYWNrZXQgZW5kPSJyIj8+xHKnrAAABWVJREFUeNrsW91rHFUUP7O7aWY3280kNdkIYrZJI8QI2UJVWpt2UglWKdb+AWrzJAh2W8SCimgfVKhItSr4IKQPPhVEVyooQbPUJrFFaBTTYD+wkqC2iyRZtdklTdZ7Zu9ud2dn5/vOTjs5MBn2fsy993fP+Z1zz0y4fD4PXhYfeFzWAPA6ANydsIgXDhwSyW1MoWrwg+PHUmp9A3fIRsa3xTh4amCj9GPx569g9I8onEu3xsnPlBdMoL8jvFJREOWzUrlXOCDeGglWFDSvW5bKtToGTNpbglx4F8qqFqi6va9ldywAiLVWxjOd4Ru6AOAMLDxGbiNCc4O465EN0NuzHvjGWwqUza3CzKV/4Lvxv2FhcRkBGCZAXHWAAOPRYPZ84vEoBJo2lDgA5ZOLG+HaEr+ZzGPKkgngIOR2/old7eKLz3XB5geaKxaPgr+xHOuxHban/ZjvfjSYAz8fqarAci0t8Onc+bHn98eErVtadM0I22F77Ef7MyVAgdg752+oqhAKPNBvlQRHyI4Kd7c3GpoVtsd+2J+1BnRFecWKe5v+s6YBSHho83p3XkkTsD8lTlYidjStKFZQExCtaEACCc+K0P4JRgQYQ3fXFA4r1vP+FckdqpmhFgAisr0Vof1ZaQB6AAB/bW8u1auYgRYAgpztjQrtL7ACoAM9QDBSs0GHhifwabC/22Unhry+hlBtDSiExDvNRIK/4Z/Xjv5ql73mSUBi9+kzhiruWxfUMoGY6UiQTHxk92ND+/v67jc8u+npC/D1N6MnyMKHGRCgQEhu/vCDf0G4Z6CirhgJFuXdX+6D7Iq/hcxjwUwckJy+MGNqkrRfkmUEqBQAGYkINQEgqH0xOzt3dXZuztDssD32w/6s/D+qdzH+VwcgW9MT6aX4fcnkKbieTutqjO2wPfZjGQJLBOfXPtCq5QZ0AYCnqVwuN3zy5Gdw+fIV1bZYj+2wvdopzB4CVHeBMhOIWcoHkMWcIMQzlfzy1Eh7W1u8r68X2trbSvXp62lCejO4+1P0KMxy8aUgiPM16DWBuKV8gEJSZK/sobjgZHkyBF1fvQKEV/srifvTK53w+7+hqiSpqaQofYiurM87R99mtshLGQ56ItUYv3T45aqyshRZygwJmhKWizcqlAg7a3IABhbkdpCe3GyJ3ZV2IhjkYWD7dhgaetRZAGrwQLkJjG3a1B0fFHdAJHKLWSPnXimoyrYdhgddnTgt3TMPvVUqy2QyMJY6DcfeOw6HDh5wDACaJBUV3SDZ/Tdw8Xuf3FOxeBaCz8dxQqEQjI5+66gW0NxAXIkDErjzTgqO9/2ZM3U3gwC1fYH1zitpwtJStsTmTgjmBi4uro9ZdoN2i5Ir0+sGjUghSXpXRW7Ap3RERJk8+6Oti7T7eeZMoPpU6KnvAzBJSi6hPNvluQ8k5AcjzwEgjwdcAcCqg0cmeW7AFQDM3XDuSx35dwOucIPLqwDXljhin3nD7q5W3Zs/9aomU4pHdVcAcE8oX9ACFRCMxgryfEAtkFwBQKNfHwhGREMD3BUJFkHAhf8p8YF1EPS+hHGVGwwHCiBklguc4IS47jvBiJTjzFMA7DGH2woAOQg+jvMeANWawE5cHQojCEUTYBUtuv4sEKHvPXycRwFwjRcoZng9C0C9hVXesO4AzM/PQ0uL9neIZvOGejkgNTl51tGF43jd3V26Fu+ECeybmPzhc3KJ5ZVbH95i6o1QldwEmBgfl66i4OKffeZpd3AAzQwPlleweLXtppela25wDYA6eQGlV+auBmDh5m7bBuN5nrutTCCXy9kyiF3PcVoDjnz40cev2zjWETcCwHn93+f/F2AAyC+9MM3whHsAAAAASUVORK5CYII=' ' class="itemThumbnail">
                       </a>
                    </div>
        
                    <div class="item_right" style="float: none; width: auto; overflow: hidden;">
                        <a href='https://dev04875.esri.com/arcgis/home/group.html?id=e090429c526b4f549b23c59fddc1f12c' target='_blank'><b>Field and Facility Maintenance</b>
                        </a>
                        <br>
                        <br><b>Summary</b>: Field Maintenance is responsible for the operations of the Water distribution system, Sewer Collection system and construction of those networks.
                        <br><b>Description</b>: Typical types of users and roles that part of this group:   Field Maintenance Manager   Asst. Field Maintenance Manager   Water/Sewer Utility I, II, III, IV   Heavy Equipment Operator   Meter Technicians/Mechanics   Building Superintendent I, II   Service Reader I,II   Warehouse ClerkCommon Task or Responsibilities of the users in this group.   Water Distribution   Valves &amp; Hydrants   Main Repair   Customer Service   Leak Investigation   Sewer Collection   Main Maintenance   Pump Operations &amp; Maintenance   TV/Video Inspection   Sewer Backup   Construction(Optional, may be under Engineering)   Main Install   Facility &amp; Grounds Maintenance   Grounds Maintenance   Regulatory Compliance   Meter Shop   Meter Reading   Inventory Control
                        <br><b>Owner</b>: portaladmin
                        <br><b>Created</b>: June 04, 2016
                        
                    </div>
                </div>
                



<div class="9item_container" style="height: auto; overflow: hidden; border: 1px solid #cfcfcf; border-radius: 2px; background: #f6fafa; line-height: 1.21429em; padding: 10px;">
                    <div class="item_left" style="width: 210px; float: left;">
                       <a href='https://dev04875.esri.com/arcgis/home/group.html?id=266b4b0653704a3697e5110d19958697' target='_blank'>
                        <img src='data:image/png;base64,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' ' class="itemThumbnail">
                       </a>
                    </div>
        
                    <div class="item_right" style="float: none; width: auto; overflow: hidden;">
                        <a href='https://dev04875.esri.com/arcgis/home/group.html?id=266b4b0653704a3697e5110d19958697' target='_blank'><b>Fire Service</b>
                        </a>
                        <br>
                        <br><b>Summary</b>: A functional group designed to provide better coordination with the local fire departments
                        <br><b>Description</b>: Typical types of users and roles that part of this group:   Engineering Technician   GIS Technician   GIS Supervisor   Fire Department(External)  Common Task or Responsibilities of the users in this group:   Out of Service Hydrants   Fire Flow   Hydrant Inspections       
                        <br><b>Owner</b>: portaladmin
                        <br><b>Created</b>: June 04, 2016
                        
                    </div>
                </div>
                



<div class="9item_container" style="height: auto; overflow: hidden; border: 1px solid #cfcfcf; border-radius: 2px; background: #f6fafa; line-height: 1.21429em; padding: 10px;">
                    <div class="item_left" style="width: 210px; float: left;">
                       <a href='https://dev04875.esri.com/arcgis/home/group.html?id=be8d8cf2725446f6bae77875fcdf5234' target='_blank'>
                        <img src='data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAEAAAABACAYAAACqaXHeAAAAGXRFWHRTb2Z0d2FyZQBBZG9iZSBJbWFnZVJlYWR5ccllPAAAAyRpVFh0WE1MOmNvbS5hZG9iZS54bXAAAAAAADw/eHBhY2tldCBiZWdpbj0i77u/IiBpZD0iVzVNME1wQ2VoaUh6cmVTek5UY3prYzlkIj8+IDx4OnhtcG1ldGEgeG1sbnM6eD0iYWRvYmU6bnM6bWV0YS8iIHg6eG1wdGs9IkFkb2JlIFhNUCBDb3JlIDUuMy1jMDExIDY2LjE0NTY2MSwgMjAxMi8wMi8wNi0xNDo1NjoyNyAgICAgICAgIj4gPHJkZjpSREYgeG1sbnM6cmRmPSJodHRwOi8vd3d3LnczLm9yZy8xOTk5LzAyLzIyLXJkZi1zeW50YXgtbnMjIj4gPHJkZjpEZXNjcmlwdGlvbiByZGY6YWJvdXQ9IiIgeG1sbnM6eG1wPSJodHRwOi8vbnMuYWRvYmUuY29tL3hhcC8xLjAvIiB4bWxuczp4bXBNTT0iaHR0cDovL25zLmFkb2JlLmNvbS94YXAvMS4wL21tLyIgeG1sbnM6c3RSZWY9Imh0dHA6Ly9ucy5hZG9iZS5jb20veGFwLzEuMC9zVHlwZS9SZXNvdXJjZVJlZiMiIHhtcDpDcmVhdG9yVG9vbD0iQWRvYmUgUGhvdG9zaG9wIENTNiAoTWFjaW50b3NoKSIgeG1wTU06SW5zdGFuY2VJRD0ieG1wLmlpZDo1NDA4RjUyNDc5NzcxMUUyODZGRkEwQ0EzMUFFRUU5NCIgeG1wTU06RG9jdW1lbnRJRD0ieG1wLmRpZDo1NDA4RjUyNTc5NzcxMUUyODZGRkEwQ0EzMUFFRUU5NCI+IDx4bXBNTTpEZXJpdmVkRnJvbSBzdFJlZjppbnN0YW5jZUlEPSJ4bXAuaWlkOjUzMDQzNjI5Nzk3NzExRTI4NkZGQTBDQTMxQUVFRTk0IiBzdFJlZjpkb2N1bWVudElEPSJ4bXAuZGlkOjUzMDQzNjJBNzk3NzExRTI4NkZGQTBDQTMxQUVFRTk0Ii8+IDwvcmRmOkRlc2NyaXB0aW9uPiA8L3JkZjpSREY+IDwveDp4bXBtZXRhPiA8P3hwYWNrZXQgZW5kPSJyIj8+wEPqmAAAAZZJREFUeNrsm7FKw1AUhpuSV9CnyFgR5zoK4lCcXMWCoy0+gmjHguLqJA4iFHGwcxEd8xT2IVIqKgGTNvcm59zE+/0QWkLK3/z5z8k9+UmQJEnLZwQI8C1A/+HEKyWuezfB8jNM7xzsnnlx8levo9/v7ZbnCLN23o3vxYmPTg+dc+YKsMT5UK4cLi5HteFUK4E4jr82TRThpAdoEUVRpH5yRTjDVfbRhgvO0MUVmzy/1IaTHmBy8GA6L7cC624Y/+bxaVKK82B/z16An5pMW/N4a9Pqj9y+fxr1gTTnznbHinP29rH2GEqgibeuKuG9A9pNXL7iAHoADkAABEAABNAfhkzW9FWiyJpeRQCbaa4s1k1zlICUA/IeI0vCBWemAFkBgjRccFICCIAAhKOEo1pWIxz9L7OALQhHC1q2Ng4gHKUH/IV0UJkF6UDWOByVDCrzOCUDWUqgibcuhiEtAQhH6QH0AL97AAIgAAIwDNmu6auEZCBrJIB0UGkzzVECUg4gHFUG4WgdSiD9RqUv4OVp3wVYCDAAeZ2u3HYr9/oAAAAASUVORK5CYII=' ' class="itemThumbnail">
                       </a>
                    </div>
        
                    <div class="item_right" style="float: none; width: auto; overflow: hidden;">
                        <a href='https://dev04875.esri.com/arcgis/home/group.html?id=be8d8cf2725446f6bae77875fcdf5234' target='_blank'><b>Gallery</b>
                        </a>
                        <br>
                        <br><b>Summary</b>: Content in this group appears on the organization gallery.
                        <br><b>Description</b>:  This group contains a collection of maps, apps and mobile configurations highlighted in the organization's gallery.  
                        <br><b>Owner</b>: portaladmin
                        <br><b>Created</b>: June 04, 2016
                        
                    </div>
                </div>
                



<div class="9item_container" style="height: auto; overflow: hidden; border: 1px solid #cfcfcf; border-radius: 2px; background: #f6fafa; line-height: 1.21429em; padding: 10px;">
                    <div class="item_left" style="width: 210px; float: left;">
                       <a href='https://dev04875.esri.com/arcgis/home/group.html?id=664aa2e58e14405aafe7f09206b5126e' target='_blank'>
                        <img src='data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAEEAAABBCAYAAACO98lFAAAAAXNSR0IArs4c6QAAAARnQU1BAACxjwv8YQUAAAAJcEhZcwAADsMAAA7DAcdvqGQAAAAadEVYdFNvZnR3YXJlAFBhaW50Lk5FVCB2My41LjEwMPRyoQAAAYJJREFUeF7t2kFOwzAQheHcki07eowq5QjZtt1ViBv0BJyBA3CA7LILnlSuLBQ7huDBTH4kC0TVJP46z5MmaRp+EEAAAQQQWCWwb5/H/zJWTTT1ZgH4eOmqH3KcIIDQTZGlEkojsDA64WEYqh/F4wBCUAkPl/extuE/ICrBxRUETYTaoiDHQxyCrkUciMPtHIZKAEG5EugOlX9/YE3QXBOIA3HgesL9/Jw4EAfiQBzkJIRrjCA0TVgJdAe6A92B7kB3cPe6/xrhcG1HGUttWu16gnZ3kMnvXp+mIX/P7d/0fYcQIISIVYRaJSyV5G+9PgcgEKntqyFoxCEGENu3uTj8pAJMIawBULkDVfqZpRjAd/db7Om10huOAZTeb5Ht+8mc3o7ZzxOaBPD9PAfCFICUlZ98+DsFYQ5AEGTCuRAmAfwCkwNhGkAg+r4fUxDmAcJ242JxnotG5H+PRVpVDRvNhLAL4D+EBQj7AAsQ2wGIQGwP4AvEdgFqWKg5BgQQSAp8AiScPGGsuHbLAAAAAElFTkSuQmCC' ' class="itemThumbnail">
                       </a>
                    </div>
        
                    <div class="item_right" style="float: none; width: auto; overflow: hidden;">
                        <a href='https://dev04875.esri.com/arcgis/home/group.html?id=664aa2e58e14405aafe7f09206b5126e' target='_blank'><b>Industrial Monitoring</b>
                        </a>
                        <br>
                        <br><b>Summary</b>: A group with maps to meet a State and Federal regulatory requirement to monitor what industries discharge into the sewer system.
                        <br><b>Description</b>: A group with maps to meet a State and Federal regulatory requirement to monitor what industries discharge into the sewer system. The Industrial Monitoring section issues permits to industries to limit the pollutants discharged into the sewer system, inspects the industries for compliance and takes action against industries that violate their permits.  Typical types of users and roles that part of this group:  Manager  Common Task or Responsibilities of the users in this group:  Industrial Waste Pretreatment ProgramIndustrial Permits and ComplianceViolationsSurveillanceEmergency Response
                        <br><b>Owner</b>: portaladmin
                        <br><b>Created</b>: June 04, 2016
                        
                    </div>
                </div>
                



<div class="9item_container" style="height: auto; overflow: hidden; border: 1px solid #cfcfcf; border-radius: 2px; background: #f6fafa; line-height: 1.21429em; padding: 10px;">
                    <div class="item_left" style="width: 210px; float: left;">
                       <a href='https://dev04875.esri.com/arcgis/home/group.html?id=8a16012300fb451ba89cccf4c2ac3549' target='_blank'>
                        <img src='data:image/png;base64,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' ' class="itemThumbnail">
                       </a>
                    </div>
        
                    <div class="item_right" style="float: none; width: auto; overflow: hidden;">
                        <a href='https://dev04875.esri.com/arcgis/home/group.html?id=8a16012300fb451ba89cccf4c2ac3549' target='_blank'><b>Lab Services</b>
                        </a>
                        <br>
                        <br><b>Summary</b>: Lab Services is responsible for testing drinking water and sampling waste water. This group includes the follow responsibility and roles at the utility.
                        <br><b>Description</b>: Lab Services functions as a support service for their environmental testing needs. The Lab also works with other government agencies to meet their testing needs. Lab Services can analyze drinking water, wastewater, surface water, groundwater, bio-solids, septage and solid samples for over 60 parameters.Typical types of users and roles that part of this group.   Manager  Common Task or Responsibilities of the users in this group.   Wastewater   FSE - FOG   Drinking Water   Trace Metals
                        <br><b>Owner</b>: portaladmin
                        <br><b>Created</b>: June 04, 2016
                        
                    </div>
                </div>
                



<div class="9item_container" style="height: auto; overflow: hidden; border: 1px solid #cfcfcf; border-radius: 2px; background: #f6fafa; line-height: 1.21429em; padding: 10px;">
                    <div class="item_left" style="width: 210px; float: left;">
                       <a href='https://dev04875.esri.com/arcgis/home/group.html?id=59bb24af570f4bf2bccd2a4d52d587c9' target='_blank'>
                        <img src='data:image/png;base64,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' ' class="itemThumbnail">
                       </a>
                    </div>
        
                    <div class="item_right" style="float: none; width: auto; overflow: hidden;">
                        <a href='https://dev04875.esri.com/arcgis/home/group.html?id=59bb24af570f4bf2bccd2a4d52d587c9' target='_blank'><b>Land Use Content</b>
                        </a>
                        <br>
                        <br><b>Summary</b>: Esri land use data with national coverage
                        <br><b>Description</b>: A catalog of Esri provided land use content with national coverage.
                        <br><b>Owner</b>: portaladmin
                        <br><b>Created</b>: June 04, 2016
                        
                    </div>
                </div>
                



<div class="9item_container" style="height: auto; overflow: hidden; border: 1px solid #cfcfcf; border-radius: 2px; background: #f6fafa; line-height: 1.21429em; padding: 10px;">
                    <div class="item_left" style="width: 210px; float: left;">
                       <a href='https://dev04875.esri.com/arcgis/home/group.html?id=7b21779b80d04e3dbf7a19ab3bbf6b5e' target='_blank'>
                        <img src='data:image/png;base64,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' ' class="itemThumbnail">
                       </a>
                    </div>
        
                    <div class="item_right" style="float: none; width: auto; overflow: hidden;">
                        <a href='https://dev04875.esri.com/arcgis/home/group.html?id=7b21779b80d04e3dbf7a19ab3bbf6b5e' target='_blank'><b>Network Operations</b>
                        </a>
                        <br>
                        <br><b>Summary</b>: Provides the latest information on the operational status of the network, including outage information, trouble reports and latest crew locations.
                        <br><b>Description</b>: Content for the network operations group.
                        <br><b>Owner</b>: portaladmin
                        <br><b>Created</b>: June 04, 2016
                        
                    </div>
                </div>
                



<div class="9item_container" style="height: auto; overflow: hidden; border: 1px solid #cfcfcf; border-radius: 2px; background: #f6fafa; line-height: 1.21429em; padding: 10px;">
                    <div class="item_left" style="width: 210px; float: left;">
                       <a href='https://dev04875.esri.com/arcgis/home/group.html?id=4f775deec43f424baab7518e54751c36' target='_blank'>
                        <img src='data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAEAAAABACAYAAACqaXHeAAAAGXRFWHRTb2Z0d2FyZQBBZG9iZSBJbWFnZVJlYWR5ccllPAAAAyRpVFh0WE1MOmNvbS5hZG9iZS54bXAAAAAAADw/eHBhY2tldCBiZWdpbj0i77u/IiBpZD0iVzVNME1wQ2VoaUh6cmVTek5UY3prYzlkIj8+IDx4OnhtcG1ldGEgeG1sbnM6eD0iYWRvYmU6bnM6bWV0YS8iIHg6eG1wdGs9IkFkb2JlIFhNUCBDb3JlIDUuMy1jMDExIDY2LjE0NTY2MSwgMjAxMi8wMi8wNi0xNDo1NjoyNyAgICAgICAgIj4gPHJkZjpSREYgeG1sbnM6cmRmPSJodHRwOi8vd3d3LnczLm9yZy8xOTk5LzAyLzIyLXJkZi1zeW50YXgtbnMjIj4gPHJkZjpEZXNjcmlwdGlvbiByZGY6YWJvdXQ9IiIgeG1sbnM6eG1wPSJodHRwOi8vbnMuYWRvYmUuY29tL3hhcC8xLjAvIiB4bWxuczp4bXBNTT0iaHR0cDovL25zLmFkb2JlLmNvbS94YXAvMS4wL21tLyIgeG1sbnM6c3RSZWY9Imh0dHA6Ly9ucy5hZG9iZS5jb20veGFwLzEuMC9zVHlwZS9SZXNvdXJjZVJlZiMiIHhtcDpDcmVhdG9yVG9vbD0iQWRvYmUgUGhvdG9zaG9wIENTNiAoTWFjaW50b3NoKSIgeG1wTU06SW5zdGFuY2VJRD0ieG1wLmlpZDo1NjI2QjM4NDc5NzcxMUUyODZGRkEwQ0EzMUFFRUU5NCIgeG1wTU06RG9jdW1lbnRJRD0ieG1wLmRpZDo1NjI2QjM4NTc5NzcxMUUyODZGRkEwQ0EzMUFFRUU5NCI+IDx4bXBNTTpEZXJpdmVkRnJvbSBzdFJlZjppbnN0YW5jZUlEPSJ4bXAuaWlkOjU2MjZCMzgyNzk3NzExRTI4NkZGQTBDQTMxQUVFRTk0IiBzdFJlZjpkb2N1bWVudElEPSJ4bXAuZGlkOjU2MjZCMzgzNzk3NzExRTI4NkZGQTBDQTMxQUVFRTk0Ii8+IDwvcmRmOkRlc2NyaXB0aW9uPiA8L3JkZjpSREY+IDwveDp4bXBtZXRhPiA8P3hwYWNrZXQgZW5kPSJyIj8+LHOOugAACNNJREFUeNrsW+1P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' ' class="itemThumbnail">
                       </a>
                    </div>
        
                    <div class="item_right" style="float: none; width: auto; overflow: hidden;">
                        <a href='https://dev04875.esri.com/arcgis/home/group.html?id=4f775deec43f424baab7518e54751c36' target='_blank'><b>Public Maps and Apps</b>
                        </a>
                        <br>
                        <br><b>Summary</b>: Unsecured public facing content.   
                        <br><b>Description</b>: This group contains a collection of maps and apps available to customers or citizens in our organization.
                        <br><b>Owner</b>: portaladmin
                        <br><b>Created</b>: June 04, 2016
                        
                    </div>
                </div>
                



<div class="9item_container" style="height: auto; overflow: hidden; border: 1px solid #cfcfcf; border-radius: 2px; background: #f6fafa; line-height: 1.21429em; padding: 10px;">
                    <div class="item_left" style="width: 210px; float: left;">
                       <a href='https://dev04875.esri.com/arcgis/home/group.html?id=b031b9f2fd7047b18cbd0fc6f8296e96' target='_blank'>
                        <img src='data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAEAAAABACAYAAACqaXHeAAAAGXRFWHRTb2Z0d2FyZQBBZG9iZSBJbWFnZVJlYWR5ccllPAAAAyRpVFh0WE1MOmNvbS5hZG9iZS54bXAAAAAAADw/eHBhY2tldCBiZWdpbj0i77u/IiBpZD0iVzVNME1wQ2VoaUh6cmVTek5UY3prYzlkIj8+IDx4OnhtcG1ldGEgeG1sbnM6eD0iYWRvYmU6bnM6bWV0YS8iIHg6eG1wdGs9IkFkb2JlIFhNUCBDb3JlIDUuMy1jMDExIDY2LjE0NTY2MSwgMjAxMi8wMi8wNi0xNDo1NjoyNyAgICAgICAgIj4gPHJkZjpSREYgeG1sbnM6cmRmPSJodHRwOi8vd3d3LnczLm9yZy8xOTk5LzAyLzIyLXJkZi1zeW50YXgtbnMjIj4gPHJkZjpEZXNjcmlwdGlvbiByZGY6YWJvdXQ9IiIgeG1sbnM6eG1wPSJodHRwOi8vbnMuYWRvYmUuY29tL3hhcC8xLjAvIiB4bWxuczp4bXBNTT0iaHR0cDovL25zLmFkb2JlLmNvbS94YXAvMS4wL21tLyIgeG1sbnM6c3RSZWY9Imh0dHA6Ly9ucy5hZG9iZS5jb20veGFwLzEuMC9zVHlwZS9SZXNvdXJjZVJlZiMiIHhtcDpDcmVhdG9yVG9vbD0iQWRvYmUgUGhvdG9zaG9wIENTNiAoTWFjaW50b3NoKSIgeG1wTU06SW5zdGFuY2VJRD0ieG1wLmlpZDo1MTU2REIwMzc5NzcxMUUyODZGRkEwQ0EzMUFFRUU5NCIgeG1wTU06RG9jdW1lbnRJRD0ieG1wLmRpZDo1MTU2REIwNDc5NzcxMUUyODZGRkEwQ0EzMUFFRUU5NCI+IDx4bXBNTTpEZXJpdmVkRnJvbSBzdFJlZjppbnN0YW5jZUlEPSJ4bXAuaWlkOjUwMzYwNjI2Nzk3NzExRTI4NkZGQTBDQTMxQUVFRTk0IiBzdFJlZjpkb2N1bWVudElEPSJ4bXAuZGlkOjUxNTZEQjAyNzk3NzExRTI4NkZGQTBDQTMxQUVFRTk0Ii8+IDwvcmRmOkRlc2NyaXB0aW9uPiA8L3JkZjpSREY+IDwveDp4bXBtZXRhPiA8P3hwYWNrZXQgZW5kPSJyIj8+ZeB4EQAABdpJREFUeNrsW19o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' ' class="itemThumbnail">
                       </a>
                    </div>
        
                    <div class="item_right" style="float: none; width: auto; overflow: hidden;">
                        <a href='https://dev04875.esri.com/arcgis/home/group.html?id=b031b9f2fd7047b18cbd0fc6f8296e96' target='_blank'><b>Tools and Add-Ins</b>
                        </a>
                        <br>
                        <br><b>Summary</b>: Collection of extensions, tools and models.
                        <br><b>Description</b>:  This group contains a collection of add-ins, tools and models used by our organization.  
                        <br><b>Owner</b>: portaladmin
                        <br><b>Created</b>: June 04, 2016
                        
                    </div>
                </div>
                



<div class="9item_container" style="height: auto; overflow: hidden; border: 1px solid #cfcfcf; border-radius: 2px; background: #f6fafa; line-height: 1.21429em; padding: 10px;">
                    <div class="item_left" style="width: 210px; float: left;">
                       <a href='https://dev04875.esri.com/arcgis/home/group.html?id=bf3a18f90281457f892756dd02be0722' target='_blank'>
                        <img src='data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAEEAAABBCAYAAACO98lFAAAAGXRFWHRTb2Z0d2FyZQBBZG9iZSBJbWFnZVJlYWR5ccllPAAAAyRpVFh0WE1MOmNvbS5hZG9iZS54bXAAAAAAADw/eHBhY2tldCBiZWdpbj0i77u/IiBpZD0iVzVNME1wQ2VoaUh6cmVTek5UY3prYzlkIj8+IDx4OnhtcG1ldGEgeG1sbnM6eD0iYWRvYmU6bnM6bWV0YS8iIHg6eG1wdGs9IkFkb2JlIFhNUCBDb3JlIDUuMy1jMDExIDY2LjE0NTY2MSwgMjAxMi8wMi8wNi0xNDo1NjoyNyAgICAgICAgIj4gPHJkZjpSREYgeG1sbnM6cmRmPSJodHRwOi8vd3d3LnczLm9yZy8xOTk5LzAyLzIyLXJkZi1zeW50YXgtbnMjIj4gPHJkZjpEZXNjcmlwdGlvbiByZGY6YWJvdXQ9IiIgeG1sbnM6eG1wPSJodHRwOi8vbnMuYWRvYmUuY29tL3hhcC8xLjAvIiB4bWxuczp4bXBNTT0iaHR0cDovL25zLmFkb2JlLmNvbS94YXAvMS4wL21tLyIgeG1sbnM6c3RSZWY9Imh0dHA6Ly9ucy5hZG9iZS5jb20veGFwLzEuMC9zVHlwZS9SZXNvdXJjZVJlZiMiIHhtcDpDcmVhdG9yVG9vbD0iQWRvYmUgUGhvdG9zaG9wIENTNiAoTWFjaW50b3NoKSIgeG1wTU06SW5zdGFuY2VJRD0ieG1wLmlpZDo4RDI4QTQzNENGRkIxMUUzQTFEREJDMEIzNzNGMjc5RCIgeG1wTU06RG9jdW1lbnRJRD0ieG1wLmRpZDo4RDI4QTQzNUNGRkIxMUUzQTFEREJDMEIzNzNGMjc5RCI+IDx4bXBNTTpEZXJpdmVkRnJvbSBzdFJlZjppbnN0YW5jZUlEPSJ4bXAuaWlkOjhEMjhBNDMyQ0ZGQjExRTNBMUREQkMwQjM3M0YyNzlEIiBzdFJlZjpkb2N1bWVudElEPSJ4bXAuZGlkOjhEMjhBNDMzQ0ZGQjExRTNBMUREQkMwQjM3M0YyNzlEIi8+IDwvcmRmOkRlc2NyaXB0aW9uPiA8L3JkZjpSREY+IDwveDp4bXBtZXRhPiA8P3hwYWNrZXQgZW5kPSJyIj8+T3j4BQAABPNJREFUeNrsm01sG0UUx9/6owlJ401KaisNwkYEqxW4+ZB6aAVKDgEhURFz5IDkM4fWlXpB4rCcuNHQO8I9VRyQeigSh0hN+LjQlJhWIlWoqBMaVXYbNxsnrdOkhPdfz0QbZwMqje1tdkZajXfXszPvl/9782bW0TY2NsjrxUeqKAgKgoKgICgICoKCoCDsWAJuGcipU2dGuTpd424mzp8/N1R9UXND2swAYlzdlue9/f3U2tq66/1cm5yk1XK5n0Fk3egOSftJIZ+vSSfhcBhVyq0xYQuElZWVmnSi6zqqPrdCiNlPlkyzJp0IF3MthGg9OvEHrHlAf26mSLNGanBlnsAzQ7tKlhzKk337aZ383oHAc/Yi6rW2CJWiJ+jewEdUfOMDKmv7vJMxfpj5dSgYf4daDr5Ewz2dFI+00RdjM9Qd8gAENh6xwGgO+k8PH++jRHclaF+dfUCBRw+IWmrgZuvrVsx1BQQBYPxot947fCRCTQEfzRUf0uUbd+nhvTvUuTxL9OLBXe9XJGFZtyghAwDvJbqskxvzJn2XnSP9zyvUXspT+NAhvrr7EBYWFlCNNzwwsgqMlw+0jGwBcP3OdMf0ZQqWKmuGcCRSk76LxSKqSw1VAgOIcQw4c1IAEC5gkuY/4X+8bMil9G9TU7UawoXqFWQj3CF9LNoR0l8IbqoA1y6mBhYpNZDG5z2dLIlgmJKzAMpMYdlRnns5Y0xyDqBLFfzBAMprTyYsFXgIQvqoTQWzHA/coIK6QUBAZAX0vhbev3ltrgIh6xkIiAVIh+0lv1QmdoVxT0Gwu4JQwYRbVq2+OrhCH7tCNNzW5EpXqJcStqjAcoXSqmP6upchJBPVEDgeeEYJ7ArJSKg5KnMDlNX1v8l8tIagmPOKEnZSgWuCYl0gxG25gS0eZN0EIfAMUk9x9TUWfSLzG7WnwLjPS2bd7gooBVs8wMwhFk14A7XI7WPPlRJ4wBmuXuEDS+AhGCaM2nFWQFnkeMAlJyBihgC4ZKMAPKsSYgIGVHCJz9OiBgjkBoMJBwgiR0BbvIof4vZSFRbIRiyonurVvBgojB2p3qzgIyPuwYjYyUTXNggFjgdf/Xz7L/4YEuqR7tBr+9qseNZovYA8rTsYwp87eIAaDv7cL+5d4eMuH+/ztOioArPiCkt8/CAMNYQi7M9LCjgpVyrhv9Jjrr7x+7R46niM7GmyLD/duk8/3rr/pTAwc7Gym+S+2YGNyYpAlatOfGwyNRx2jT7VNC3+eldoGwDkBt9OzVPAhz80/Q4IIpbI9mmhimT1ylLAzcgp12FcaHuOj88cxhUTbdud2u6oBG6IC6aQqXyAwYFu5O0jEfolV0RwmxD3s3Lwx2IH9OaAz7qPdwkyP8DLlEk+hg+H6fq8ibbX+PJZGCtiDHafBzGTjN0sYLfpgng24kEKG7Nv9nSG4EpXc0X7uGCgAdd7q6cTCgNsOa5xoTaD7+lNQT+NTec321bDcITw8eCrlnSxB4h5HgaxkdZLEhRskMI4DAz34P9syGbww4Bm8iViA6z78gWLdAnsKmGWwLNhgIwfSKnZUAsWSpSfyQBI5hroT44LagM4e1sB2VIe2mLMUpWyLb4jYs+/Q/jk3cO0V8vn39/cBkH9jlFBUBAUBAVBQVAQFAQFQUH4HztLSC29VDT1v9LKHRQEBUFB2Fr+EWAA8vMMk0RMtW8AAAAASUVORK5CYII=' ' class="itemThumbnail">
                       </a>
                    </div>
        
                    <div class="item_right" style="float: none; width: auto; overflow: hidden;">
                        <a href='https://dev04875.esri.com/arcgis/home/group.html?id=bf3a18f90281457f892756dd02be0722' target='_blank'><b>Wastewater Treatment</b>
                        </a>
                        <br>
                        <br><b>Summary</b>: The Wastewater Treatment Division operates and maintains the treatment plant.   
                        <br><b>Description</b>: The Wastewater Treatment Division operates and maintains the treatment plant. The division protects the health of the community and environment by reclaiming wastewater from residential, commercial, and industrial sources.  Typical types of users and roles that part of this group:Superintendent of Water Pollution ControlWastewater Treatment Plant Operator I, II, I  Utility I, II  Common Task or Responsibilities of the users in this group:  Identify and define prohibited wastes;  Develop local limits on specific pollutants;  Obtain access to facilities for inspections and sampling;  Require non-residential dischargers to provide an adequate sampling location;  Require treatment of non-domestic wastes to meet discharge standards;  Require non-residential dischargers to obtain discharge permits;  Require non-residential dischargers to monitor their discharge and submit reports;  Take enforcement action for non-compliance with the program regulations.
                        <br><b>Owner</b>: portaladmin
                        <br><b>Created</b>: June 04, 2016
                        
                    </div>
                </div>
                



<div class="9item_container" style="height: auto; overflow: hidden; border: 1px solid #cfcfcf; border-radius: 2px; background: #f6fafa; line-height: 1.21429em; padding: 10px;">
                    <div class="item_left" style="width: 210px; float: left;">
                       <a href='https://dev04875.esri.com/arcgis/home/group.html?id=74b82316cc25455586df51420880d56d' target='_blank'>
                        <img src='data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAEEAAABBCAYAAACO98lFAAAAGXRFWHRTb2Z0d2FyZQBBZG9iZSBJbWFnZVJlYWR5ccllPAAAAyRpVFh0WE1MOmNvbS5hZG9iZS54bXAAAAAAADw/eHBhY2tldCBiZWdpbj0i77u/IiBpZD0iVzVNME1wQ2VoaUh6cmVTek5UY3prYzlkIj8+IDx4OnhtcG1ldGEgeG1sbnM6eD0iYWRvYmU6bnM6bWV0YS8iIHg6eG1wdGs9IkFkb2JlIFhNUCBDb3JlIDUuMy1jMDExIDY2LjE0NTY2MSwgMjAxMi8wMi8wNi0xNDo1NjoyNyAgICAgICAgIj4gPHJkZjpSREYgeG1sbnM6cmRmPSJodHRwOi8vd3d3LnczLm9yZy8xOTk5LzAyLzIyLXJkZi1zeW50YXgtbnMjIj4gPHJkZjpEZXNjcmlwdGlvbiByZGY6YWJvdXQ9IiIgeG1sbnM6eG1wPSJodHRwOi8vbnMuYWRvYmUuY29tL3hhcC8xLjAvIiB4bWxuczp4bXBNTT0iaHR0cDovL25zLmFkb2JlLmNvbS94YXAvMS4wL21tLyIgeG1sbnM6c3RSZWY9Imh0dHA6Ly9ucy5hZG9iZS5jb20veGFwLzEuMC9zVHlwZS9SZXNvdXJjZVJlZiMiIHhtcDpDcmVhdG9yVG9vbD0iQWRvYmUgUGhvdG9zaG9wIENTNiAoTWFjaW50b3NoKSIgeG1wTU06SW5zdGFuY2VJRD0ieG1wLmlpZDo4REI3M0IyNUNGRkIxMUUzQTFEREJDMEIzNzNGMjc5RCIgeG1wTU06RG9jdW1lbnRJRD0ieG1wLmRpZDo4REI3M0IyNkNGRkIxMUUzQTFEREJDMEIzNzNGMjc5RCI+IDx4bXBNTTpEZXJpdmVkRnJvbSBzdFJlZjppbnN0YW5jZUlEPSJ4bXAuaWlkOjhEMjhBNDM2Q0ZGQjExRTNBMUREQkMwQjM3M0YyNzlEIiBzdFJlZjpkb2N1bWVudElEPSJ4bXAuZGlkOjhEQjczQjI0Q0ZGQjExRTNBMUREQkMwQjM3M0YyNzlEIi8+IDwvcmRmOkRlc2NyaXB0aW9uPiA8L3JkZjpSREY+IDwveDp4bXBtZXRhPiA8P3hwYWNrZXQgZW5kPSJyIj8+hiZiawAAA4hJREFUeNrsmj9oE1Ecx18kgSYWkyIVI0JUWqUt2IDo2hsdhKaDnYSmu2CXW5xucsmSwcHNFJy6NIJDN5ulQ0SSCjZoim2omECRJm1NCwnE3y/+Itf0+s+8l1zj78Hj5S53797v877f37vLxVGr1cT/Xi4ILgyBIdgFwpvnD8NYOzkGR6cSIwSuQRPt7788itubmz+XoZl58mJhseshQPA+DN7lck0F746IQOB6fX8u912kP30WlUpllmAUuxICAJiBxhgcuOkdHrojXC7nge8rlapYyXwR2dW1Eh4HIKJdA8EsfZx9r/fSsceXStt1VbTLIkohNKR/0eOZGh0dFtf8V890PlpkJfNV/CqXlVpEGYSG9IeHbnsHB24dkv5pC1oku/oNYSiziHQIZunfvxcUHo9bSr/l8p748DGtxCLSILQq/dOWH/mCWF5ekWoRKRAa0ofqbfMSL8UiUu0QiURqk5OTf7cDgYBFsstJudbc3JzQdd3Bzw78AMUQGAJDYAgMgSEwBIbAEBgCQ2AIDIEhMASGwBAYQqdLyz+0Jp8+ssX/fR68fOdgJXRSCc2K2Husi97eXqWDzufz4srCq5YVoEwJyWSytLGxoST4arUqMpkM1pzMflXYQctms4lUKiX29/eldVosFhEwqgBfvwVlDtgpm4Cu62kEEYlEwktLS4bf7w9AFT6f75+lXygUxNbWVgI2Deh/8Y/9EvaFYIIRgyaGMCCQUE9Pz3hfX18dhtvttoSCct/d3RU7Ozv1mYfAt2HfPPbTCP5cJEYo01DjkLAOvC0GGBi1RjVItfkFLnp9HSoGvGgVOFwH+wlBfS0rMaqAUKIWlRCFQa5L6v8GNPj2O0y7vLZdHaDgYA2arTUYfByq1kLwGvaBfVGfBl3DvjmBbID/F4jC4EM0e+/hc44COGSVYySPx+P7fcyCE3Be3HSM/RMjAcFBx0nKhgmOpVUsJI/nG7Is1REIJhgYRJhmOEyBPoPttwRG0L5xSo4ILHaSYs4VhCOsEqbP46aEOg3HxNr97OBs9wVN1gjRrllqQwRHa4cFOgLBlCTHrCTfZBVcVRKUN+LnGkJTYJZZ/oRVZZ5WlajKHOFsk+TPlOUtVpV6pfsF6VZRcceYMEleygweoagxWXeMquwwIdPLR1jF1neMmso8Y7KKtN82+TdGhsAQGIKSJZKVwBAYAkPolvJbgAEAeezXug+W24UAAAAASUVORK5CYII=' ' class="itemThumbnail">
                       </a>
                    </div>
        
                    <div class="item_right" style="float: none; width: auto; overflow: hidden;">
                        <a href='https://dev04875.esri.com/arcgis/home/group.html?id=74b82316cc25455586df51420880d56d' target='_blank'><b>Water Treatment</b>
                        </a>
                        <br>
                        <br><b>Summary</b>: The Water Treatment Division operates and maintains treatment plants, elevated water storage tanks, reservoirs and pumping stations.
                        <br><b>Description</b>: The Water Treatment Division operates and maintains the treatment plant. The treatment process, which includes lime softening, dual media filtering, and chlorination and fluoridation, supplies a high quality, finished water that is unsurpassed in the region.  Typical types of users and roles that part of this group:  Superintendent of Water Pollution ControlWater Treatment Plant Operator I, II, IUtility I, II  Common Task or Responsibilities of the users in this group:  Boil Water NoticesConsumer Confidence ReportWater Treatment
                        <br><b>Owner</b>: portaladmin
                        <br><b>Created</b>: June 04, 2016
                        
                    </div>
                </div>
                



<div class="9item_container" style="height: auto; overflow: hidden; border: 1px solid #cfcfcf; border-radius: 2px; background: #f6fafa; line-height: 1.21429em; padding: 10px;">
                    <div class="item_left" style="width: 210px; float: left;">
                       <a href='https://dev04875.esri.com/arcgis/home/group.html?id=2daf11356d884889b53cc61dbb560541' target='_blank'>
                        <img src='data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAEAAAABACAYAAACqaXHeAAAAGXRFWHRTb2Z0d2FyZQBBZG9iZSBJbWFnZVJlYWR5ccllPAAAAyRpVFh0WE1MOmNvbS5hZG9iZS54bXAAAAAAADw/eHBhY2tldCBiZWdpbj0i77u/IiBpZD0iVzVNME1wQ2VoaUh6cmVTek5UY3prYzlkIj8+IDx4OnhtcG1ldGEgeG1sbnM6eD0iYWRvYmU6bnM6bWV0YS8iIHg6eG1wdGs9IkFkb2JlIFhNUCBDb3JlIDUuMy1jMDExIDY2LjE0NTY2MSwgMjAxMi8wMi8wNi0xNDo1NjoyNyAgICAgICAgIj4gPHJkZjpSREYgeG1sbnM6cmRmPSJodHRwOi8vd3d3LnczLm9yZy8xOTk5LzAyLzIyLXJkZi1zeW50YXgtbnMjIj4gPHJkZjpEZXNjcmlwdGlvbiByZGY6YWJvdXQ9IiIgeG1sbnM6eG1wPSJodHRwOi8vbnMuYWRvYmUuY29tL3hhcC8xLjAvIiB4bWxuczp4bXBNTT0iaHR0cDovL25zLmFkb2JlLmNvbS94YXAvMS4wL21tLyIgeG1sbnM6c3RSZWY9Imh0dHA6Ly9ucy5hZG9iZS5jb20veGFwLzEuMC9zVHlwZS9SZXNvdXJjZVJlZiMiIHhtcDpDcmVhdG9yVG9vbD0iQWRvYmUgUGhvdG9zaG9wIENTNiAoTWFjaW50b3NoKSIgeG1wTU06SW5zdGFuY2VJRD0ieG1wLmlpZDo1NjI2QjM4Qzc5NzcxMUUyODZGRkEwQ0EzMUFFRUU5NCIgeG1wTU06RG9jdW1lbnRJRD0ieG1wLmRpZDpGODI5RkFDMjc5NzcxMUUyODZGRkEwQ0EzMUFFRUU5NCI+IDx4bXBNTTpEZXJpdmVkRnJvbSBzdFJlZjppbnN0YW5jZUlEPSJ4bXAuaWlkOjU2MjZCMzhBNzk3NzExRTI4NkZGQTBDQTMxQUVFRTk0IiBzdFJlZjpkb2N1bWVudElEPSJ4bXAuZGlkOjU2MjZCMzhCNzk3NzExRTI4NkZGQTBDQTMxQUVFRTk0Ii8+IDwvcmRmOkRlc2NyaXB0aW9uPiA8L3JkZjpSREY+IDwveDp4bXBtZXRhPiA8P3hwYWNrZXQgZW5kPSJyIj8+pYgXrgAAAeRJREFUeNrsWz1Lw1AUzZP8Cwd3yaa/wFkQQfdOTuIk4lCllA4iXQQnp+4KIhRx8R/oFtwd/B2RCEobU9qX3I9zyT1DS0t7kndy3815h5dQFEXSZaTlSwiBjPBm+GpC0ZOLnfAnADUOe1vQg7+fvM9XwGA4an3VBpf9YHYKlDg/O21McnU9NtsD1pKOo/MCpFIHupvcRv/nqHdMzrFUgNn53KYv1CGGb1FfoeCIroDHp2nt9/t7u6QC5Xn+855lmRjHSgJQDxS6B1CXfQzaXPmmHKnkAH/LU5tDTYCYqzN9fmHjUBOg2qQ+3w5qf7Ox/cDOoSZAFeub/bnPXx8jUQ5xATQaHZQVLsu3bSOj4ICZAk1KnpJD1ApXy3fVRsXBoWKFKXID6uxBzAovW5VJcUBbYQ9EXACA1SAXzCRCnEBMhKCmgIZL9B6AdDIQiRCnFTabCFFZYbOJEGUqDJ8ISVthT4SUONRug54IVQCXCElOATOJEBXMJkIUQE2EfC0gaYXNLoakNkjACkAxUDOJEGfZeyIE6BL9LoB0Mr5HSIhDTQDfI5T4HqF/gEuEJDdIaHKoWWGN/cAQVngR4BIh7hWgJ0IAHCoCeCIECl8Mcc0tUwJYfeoTtgnOPpuLjtD1x+e/BRgA8Kodycf8/s0AAAAASUVORK5CYII=' ' class="itemThumbnail">
                       </a>
                    </div>
        
                    <div class="item_right" style="float: none; width: auto; overflow: hidden;">
                        <a href='https://dev04875.esri.com/arcgis/home/group.html?id=2daf11356d884889b53cc61dbb560541' target='_blank'><b>Web App Templates</b>
                        </a>
                        <br>
                        <br><b>Summary</b>: Collection of web application templates.
                        <br><b>Description</b>: This group contains a collection of web application templates used in our organization. The application templates are used to exposed web maps as applications.  
                        <br><b>Owner</b>: portaladmin
                        <br><b>Created</b>: June 04, 2016
                        
                    </div>
                </div>
                
