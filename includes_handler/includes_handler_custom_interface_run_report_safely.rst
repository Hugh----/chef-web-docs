.. The contents of this file are included in multiple topics.
.. This file should not be changed in a way that hinders its ability to appear in multiple documentation sets.


The ``run_report_safely`` method is used to run the report handler, rescuing and logging errors that may arise as the handler runs and ensuring that all handlers get a chance to run during the |chef client| run (even if some handlers fail during that run). In general, this method should never be used as an interface in a custom handler unless this default behavior simply must be overridden.

.. code-block:: ruby

   def run_report_safely(run_status)
     run_report_unsafe(run_status)
   rescue Exception => e
     Chef::Log.error('Report handler #{self.class.name} raised #{e.inspect}')
     Array(e.backtrace).each { |line| Chef::Log.error(line) }
   ensure
     @run_status = nil
   end
