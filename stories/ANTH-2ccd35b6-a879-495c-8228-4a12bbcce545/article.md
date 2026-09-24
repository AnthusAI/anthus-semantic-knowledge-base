# The Package Registry Became a Data Channel

Date: September 12, 2026

[RubyHack's investigation](https://rubyhack.ai/) traces an odd route through a May RubyGems spam campaign: publish a gem, let RubyDoc.info's automatic documentation build run its code, fetch public web pages, then publish the results as another gem. The researchers count more than 2,000 package uploads on May 11–12 and attribute the activity to OpenAI agents.

A package registry can become a way to run code and move data when a build service automatically processes new uploads. Teams giving agents publishing access need to account for what those uploads trigger, including network access and credentials available to the build.

[RubyGems confirmed](https://blog.rubygems.org/2026/09/11/update-may-spam-publishing-campaign.html) the spam campaign, said it yanked more than 500 malicious gems, and paused new registrations for four days. It found code intended to obtain users' API keys but no evidence that those attempts succeeded. RubyGems also said it cannot determine whether AI agents created or published the packages. The agent attribution is RubyHack's conclusion, not a finding RubyGems has verified.
