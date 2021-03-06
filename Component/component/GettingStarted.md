## How to integrate
To use the library in your project, you should to do a few simple steps:

1. Add a reference to mTouch-PDFReader library in your project. Or install mThouch-PDFReader component from Xamarin Component Store.
2. Register global managers in *AppDelegate* class:
<pre><code>
public partial class AppDelegate : UIApplicationDelegate 
{   
  public AppDelegate()
  {
    var builder = new ContainerBuilder();
    builder.RegisterType<MyDocumentBookmarksManager>().As<IDocumentBookmarksManager>().SingleInstance();
    builder.RegisterType<MyDocumentNoteManager>().As<IDocumentNoteManager>().SingleInstance();
    builder.RegisterType<SettingsManager>().As<ISettingsManager>().SingleInstance();
    MgrAccessor.Initialize(builder);
  }
}
</code></pre>
3. To open PDF document, use the following code:
<pre><code>
// Set unique document id, document name (will be used as title) and document full file path
var docId = 1;
var docName = "My book";
var docPath = Path.Combine(Environment.GetFolderPath(Environment.SpecialFolder.Personal), "MyBook.pdf");
  
// Create Document View controller and pust it in Navigation controller, for example 
var docVC = new DocumentVC();   
NavigationController.PushViewController(docVC, true);
</code></pre>

> That's it! The PDF document will be opened with default settings:)

## How to use Bookmarks and Note manager
The library contains the simple versions of Bookmarks and Note manager. In demo application the all data stored in the memory, and after closing application is losing. But you can inherit these managers and store data in a file or Sqlite db. To do it, you should inherit DocumentBookmarksManager (for bookmarks) and DocumentNoteManager (for notes) managers:
<pre><code>
public class MyDocumentBookmarksManager : DocumentBookmarksManager
{
  public override List<DocumentBookmark> GetAllForDocument(int docId)
  {
    // This method must return all bookmarks objects for the document with id=docId
  }
  public override void Save(DocumentBookmark bookmark)
  {
    // This method for saving bookmark object
  }
  public override void Delete(int bookmarkId)
  {
    // This method for deleting bookmark by id
  }
}

public class MyDocumentNoteManager : DocumentNoteManager
{
  public override DocumentNote Load(int docId)
  {
    // This method for loading note for document, by docId
  }
  public override void Save(DocumentNote note)
  {
    // This method for saving note object
  }
}
</code></pre>

## How to use Settings
To use Settings View Controller, just create a SettingsVC object and add it as subview to your View Controller:
<pre><code>
var settingsVC = new SettingsTableVC();
settingsVC.View.Frame = View.Bounds;
View.AddSubview(settingsVC.View);
</code></pre>