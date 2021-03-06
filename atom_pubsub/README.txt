
	atom_pubsub - the Atom PubSub tunnel

	Author: Eric Cestari <eric@ohmforce.com> http://www.cestari.info/
    Licensed under the same terms as ejabberd (GPL 2)
	Requires: ejabberd 2.0.3 or newer.
	Does NOT work with ejabberd 13 or newer.


	DESCRIPTION
	-----------

The atom_pubsub module provides access to all PEP nodes via an AtomPub interface.
Also gives access to tune, mood and geoloc nodes if they exist.

urn:xmpp:microblog is not a XEP yet, but its latest incarnation can be found here :
http://www.xmpp.org/extensions/inbox/microblogging.html

AtomPub RFC : http://bitworking.org/projects/atom/rfc5023.html

For more information refer to:
http://www.cestari.info/2008/6/19/atom-pubsub-module-for-ejabberd
http://www.cestari.info/2008/9/12/atom_pubsub-dead-long-live-atom_microblog


	USAGE
	-----

URL for the service document for a given user@domain is :
http://<server>:5280/pep/<domain>/<user>

The atom_pubsub module provides access to all nodes below the /home/server/user tree.
SVC document : http://<server>:5280/pubsub/<domain>/<user>


# Configuring your PEP nodes 

For your PEP nodes to be published, you need to activate persistence.
Two ways:

A) For existing nodes :
<iq type='set'
    id='config2'>
  <pubsub xmlns='http://jabber.org/protocol/pubsub#owner'>
    <configure node='http://jabber.org/protocol/tune'> <!-- for user-tune -->
      <x xmlns='jabber:x:data' type='submit'>
        <field var='FORM_TYPE' type='hidden'>
          <value>http://jabber.org/protocol/pubsub#node_config</value>
        </field>
        <field var='pubsub#persist_items'><value>1</value></field>
        <field var='pubsub#max_items'><value>1</value></field>
      </x>
    </configure>
  </pubsub>
</iq>

B) for new nodes :
It's better to change src/mod_pubsub/node_pep.erl in the options() function:
  {persist_items, true},
  {max_items, 1},
All future PEP nodes will be published with those parameters by default.


# What you get

Full caching support with Etag, Conditional Get, etc.

Authentication is required for writing, updating via Atom. Use your full JID as username for authentication.

It expects the payload to be an atom entry, but does not enforce it.

However, it has to be well formed XML.


# Can I have it with OpenFire and Epeios ?

That's not possible. atom_microblog needs direct access to the pubsub structures.


	WHAT'S NEXT?
	------------

* Better understanding of Atom entries. have better links, implement reply-to 

* Adding a local friend collection for personal use

* But that may be implemented in mod_pubsub/node_microblog.erl (it will arrive soon)


	THANKS
	------

* johnny_ for testing and giving feedback.
* badlop and C Romain from ProcessOne for feeding ejabberd with my patches, making this code work.

