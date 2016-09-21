
Hardening AWS Environments
==============================================================================

and
===

Automating Incident Response
===============================

Andrew Krug & Alex McCormack

@andrewkrug   @amccomarck

----------------

Before we get started
=======================

.. image:: static/pwnie.png

ImageCredit: OddDuckart

http://bit.ly/2cl6qJ2

-----------------

About Us
==========

.. image:: static/team.png

-----------------

Important Things
==================

* We are not a corporation
* No one pays us to do this
* Everything we're going to talk about is FOSS

Other important things
-----------------------

* We all have day jobs on the ThreatResponse Team
* These are our opinions not the opinion of our employer.

-----------------

Why AWS Incident Response
============================

.. image:: static/why.jpg
   :align: center

-----------------

.. image:: static/sowhat2.jpg
   :align: center



-----------------

This year in AWS Security
===========================

-----------------

BlackHat 2016 Talks
===========================

-----------------

Account Jumping, Post Infection Persistence, and Lateral Movement in AWS
==========================================================================

Dan Amiga and Dor Knafo
------------------------

.. image:: static/fireglass.png
   :align: center

http://ubm.io/2dfeStx


-----------------

Access Keys will kill you before you kill the password
=======================================================

Loic Simon
----------

.. image:: static/bh2016.png
   :align: center

http://ubm.io/2czdg9S

-----------------

Us of Course!
========================


Hint : You're in this talk.
----------------------------

.. image:: static/awsir.jpg
   :align: center

http://www.threatresponse.cloud

------------------

Significant Blogs
===========================

.. image:: static/danielgrzelak.png
   :align: center

-----------------

Things we're gonna do!
===============================

1. Examine Attacks Against AWS Accounts
2. Show you the ThreatResponse Toolkit
3. Talk about other OSS Project
4. Release a new type of attack tool

( not necessarily in that order )

-----------------

Common Types of Attacks
=================================

1. Host Based Compromise
2. Key Based Compromise

.. image:: static/hk-kc.png
   :align: center

-----------------

How does this happen?
=================================

* Zero-Days
* Default Credentials
* Carelessness

**They do happen!  This can happen to you.**

-----------------

HC can become KC
====================

Ever heard of the metadata service?
-------------------------------------

-----------------

**MetaData Service**

.. code-block:: bash

    https://aws.amazon.com/amazon-linux-ami/2016.03-release-notes/
    13 package(s) needed for security, out of 26 available
    Run "sudo yum update" to apply all updates.
    [ec2-user@ip-172-31-37-29 ~]$ curl http://169.254.169.254/latest/meta-data/
    ami-id
    ami-launch-index
    ami-manifest-path
    block-device-mapping/
    hostname
    iam/
    instance-action
    instance-id
    instance-type
    local-hostname
    local-ipv4
    mac
    metrics/
    network/
    placement/
    profile
    public-hostname
    public-ipv4
    public-keys/
    reservation-id
    security-groups


-----------------

**Determine Instance Profile**

.. code-block:: bash

    curl http://169.254.169.254/latest/meta-data/iam/info
    {
      "Code" : "Success",
      "LastUpdated" : "2016-09-21T17:00:07Z",
      "InstanceProfileArn" : "arn:aws:iam::671642278147:instance-profile/\
      cloudresponse_workstation-cr-16-080120-e5c0-us-west-1",
      "InstanceProfileId" : "AIPAJJWTONXQ7CLMRENCO"
    }

-----------------

**Once you know the role name**

.. code-block:: bash

    curl http://169.254.169.254/latest/meta-data/iam/\
    security-credentials/cloudresponse_workstation-cr-16-080120-e5c0-us-west-1
    {
      "Code" : "Success",
      "LastUpdated" : "2016-09-21T17:00:55Z",
      "Type" : "AWS-HMAC",
      "AccessKeyId" : "ASIAJDU**********REDACTED",
      "SecretAccessKey" : "q7bVQVlV+9/ktjWgh5******REDACTED",
      "Token" : "FQoDYXdzEGIaDGlEkwRSH8hHG+Oz***********REDACTED",
      "Expiration" : "2016-09-21T23:05:14Z"
    }

Winning!
================================



-----------------

HC
=================

.. image:: static/hc.png
   :align: center

-----------------

KC
=============

.. image:: static/kc-cropped.png
   :align: center

-----------------

KC
=================

.. image:: static/kc.png
   :align: center

-----------------------------

The AWS Security ECO System
=============================

Basically all you need is:

1. Word about a Cloud
2. Action or a Place
3. ( Optional a thing to operate on )

You too can make Product Madlibs
--------------------------------


-----------------

Attack Time!
==============================

Trivia Question
----------------

Who Said: "Defense without Offense is after all just Compliance."

---------------------------

A: "Dan Kaminsky in Read My Lips: Let’s Kill 0Day"

.. image:: static/kaminsky.jpg
   :align: center

-----------------------------

Attack Scenario
=============================

Imagine .... once upon a time

-----------------------------

Attack Retrospective
=============================

-----------------

What is ThreatResponse?
=============================

.. image:: static/tool-release.png
    :align: center

------------------------------

ThreatResponse in Action
================================

------------------------------

What just happened?
================================

------------------------------

So what?
==============================

.. image:: static/projects.png
    :align: center

------------------------------

AWS Advanced Attacks
================================

* Logging Disruption
* STS Persistence
* *New* Super Cool API Gateway Persistence

------------------------------

PSA : GroundRules
=============================

.. image:: static/boring.jpg
    :align: center

Non-Boring Material Ahead!
----------------------------

------------------------------

Logging Disruption
===============================================

Three Variations of This
---------------------------

1. Just Stop Trail - Boring
2. Stop Regional Logging or Global Logging - Less Boring
3. Make CloudTrail operate but logs are unreadable - Best!!

------------------------------

The Cool Attack
======================================

This is your CloudTrail
------------------------------------

.. image:: static/normalcloudtrail.png
    :align: center


------------------------------

**This is your CloudTrail on Crypto**


.. image:: static/badcloudtrail.png
    :align: center

----------------------------------

When the attack happens...
==============================================

.. image:: static/moneyfire.jpg
    :align: center

The “bypass-policy-lockout-safety-check” flag allows you the make the key’s
policy immutable after creation, making logging just an exercise in lighting
money on fire with disk consumption. You can’t say Amazon didn’t warn you!
- @danielgrzelak


----------------------------------

.. image:: static/sowhat.png
    :align: center

1. Requires a high level of privilege
2. Handy for remaining undetected
3. Not necessarily undetectable...

----------------------------------

Not Normal Activities Here
==============================

.. image:: static/moon.gif
    :align: center

1. Creating KMS Keys with this weird policy
2. Calling update trail on your cloudtrail

----------------------------------

http://bit.ly/2cnpTsK
=================================

There's an article about this type of detection.
----------------------------------------------------

.. image:: static/doda.png
    :align: center

----------------------------------

CloudWatch Event Pipelines
=========================================

For the win
-------------------

.. image:: static/cloudwatch.png
    :align: center

----------------------------------


Video of CloudWatch Pipeline
================================

.. raw:: html

    <video width="824" height="376" controls>
      <source src="videos/advcloudtrail.webm">
    Your browser does not support the video tag.
    </video>

----------------------------------

STS Attacks
======================================

.. image:: static/sts.png
    :align: center

----------------------------------


Why make a backdoor tool?
==============================

Trivia Question
----------------

Who Said: "It was once my job to think as Dark Wizards do?"

-----------------------------

A: "Mad Eye Moody"

.. image:: static/moody.gif

-----------------------------

Mad King Demo
=================================

.. image:: static/madking.png

------------------------------

Just Imagine
============================

.. image:: static/story1.jpg

You're working in the magical land of Cosnovion.


------------------------------

Then bad things happen
============================

.. image:: static/story2.jpg

One of your developers leaks a super privileged access key...

------------------------------

You save the day?
============================

.. image:: static/story3.jpg

They said give us some money or else.  Boss asks you to clean the account.
And you do! You even revoked STS Tokens!


-------------------------------

Attackers end your company
============================

.. image:: static/story4.jpg

Attackers end your company through a super cool new type of persistence.

--------------------------------

Fin
============================

.. image:: static/story5.jpg

The End


--------------------------------

So what?
=================================

Let's look at the MadKing
--------------------------

------------------------------

How do we even begin to protect ourselves?
===========================================

------------------------------

No less than:
===========================================

Three Dumb Clouds
------------------

.. image:: static/dev-in-aws.png
    :align: center

Is this three dumb clouds?

--------------

Other Projects
===========================================

------------------------------

Project Comparison
===========================================

.. image:: static/comparison.png
    :align: center


------------------------------

Want more information?
==========================================

Subscribe to our mailing list
--------------------------------

http://www.threatresponse.cloud

------------------------------

Future Features of Our Tools
==========================================

.. image:: static/features.png
    :align: center

------------------------------

Thank Yous and Announcements
==========================================

* Amazon Web Services Security
      Don Bailey, Henrik Johansson, Zack Glick
* DerbyCon Staff
* Toni De la Fuente
* Team Who Couldn't Be with Us Today


------------------------------

Don't let me forget to take questions...
==========================================

------------------------------

Srsly any questions? ...
==========================================
