Loop is composed of groups and objects. These groups and objects are
organized by a object management hierarchy. Certain groups are linked to
objects based on their inheritance. Looking at the diagram below, you
can see that depending on the level of a group decides on what objects
or control they have. Groups only have control over the objects and
groups that are nested underneath it. |Object Management Hierarchy|

Root
----

.. figure:: https://s3-us-west-1.amazonaws.com/github-photos/DeveloperDocs/Loop/omanobjectdiagram-root.png
   :alt: Object Management Hierarchy

   Object Management Hierarchy

For this instance of Loop, *Root* is at the base of the object
management hierarchy. *Root* has total administration control of all the
objects and groups underneath it. In the current instance of Loop, the
*Root* user is BlueCats. This allows us to manage all of groups and
objects.

Use Case: This could allow the Managers of the company complete control
over all of the objects and groups so that they can monitor and have
complete administrative control.

For example, there is a *Place::Object* only under *Root*. No other
Group has access of that Place::Object besides *Root*. In another
example, the *sbVehicle* is under *SITE B (SB)* but because it is under
*Root*, *Root* still has has control over that object.

Tenant
------

.. figure:: https://s3-us-west-1.amazonaws.com/github-photos/DeveloperDocs/Loop/omanobjectdiagram-tenant.png
   :alt: Object Management Hierarchy

   Object Management Hierarchy

*Tenants* are the first group nested within *Root*. *Tenants* have total
administration control of all the objects and groups underneath it. Each
*Tenant* has seperate control of its own objects and groups. *Tenants*
allow a seperation of control and allows you to provide subgroupings of
users and objects without giving complete administration control.

Use Case: This could allow a lower level managers or different offices
of the company control over the objects and groups that are under their
authority and nothing more.

In this example, *Tenant A (TA)* has control of *taPERSON* and all of
the objects and groups nested under it. However, *Tenant B (TB)* has no
control of *taPERSON* or *cbMaterial*.

Customer
--------

.. figure:: https://s3-us-west-1.amazonaws.com/github-photos/DeveloperDocs/Loop/omanobjectdiagram-customer.png
   :alt: Object Management Hierarchy

   Object Management Hierarchy

*Customers* are the group nested within *Tenant*. *Customers* have total
administration control of all the objects and groups underneath it.
*Customers* allow a seperation of control and allows you to provide
subgroupings of users and objects.

Use Case: This does not have to be an actual customer but could allow
for a subgrouping underneath the Tenant. This could be a specific
department or specific area of your company.

In this example, *Customer A (CA)* has control of *SITE B (SB)* and all
of the objects and groups nested under it. However, *Customer B (CB)*
has no control of *SITE B (SB)* or *sbVEHICLE*.

Site
----

.. figure:: https://s3-us-west-1.amazonaws.com/github-photos/DeveloperDocs/Loop/omanobjectdiagram-site.png
   :alt: Object Management Hierarchy

   Object Management Hierarchy

*Sites* are the group nested within *Customers*. *Sites* have total
administration control of all the objects and groups underneath it.
*Sites* allow a seperation of control and allows you to provide
subgroupings of users and objects.

Use Case: This does not have to be an actual customer but could allow
for a subgrouping underneath the *Customer*. This could be a specific
department or specific area of your company.

In this example, *Customer A (CA)* has control of *SITE B (SB)* and all
of the objects and groups nested under it. However, *Customer B (CB)*
has no control of *SITE B (SB)* or *sbVEHICLE*.

Other Groupings
---------------

These groupings and this naming convention is just one possible way to
group objects. This grouping follows the basic administration for
companies with different tenants and customers. It is possible to group
the objects in many different ways. This use case is to show you the
customization and the power of Loop.

.. |Object Management Hierarchy| image:: https://s3-us-west-1.amazonaws.com/github-photos/DeveloperDocs/Loop/omanobjectdiagram.png

