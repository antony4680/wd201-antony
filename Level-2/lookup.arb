def get_command_line_argument
  # ARGV is an array that Ruby defines for us,
  # which contains all the arguments we passed to it
  # when invoking the script from the command line.
  # https://docs.ruby-lang.org/en/2.4.0/ARGF.html
  if ARGV.empty?
    puts "Usage: ruby lookup.rb <domain>"
    exit
  end
  ARGV.first
end

# `domain` contains the domain name we have to look up.
domain = get_command_line_argument

# File.readlines reads a file and returns an
# array of string, where each element is a line
# https://www.rubydoc.info/stdlib/core/IO:readlines
dns_raw = File.readlines("zone")

def parse_dns(dns_raw)
  record_types = ["A", "CNAME"]
  dns_records = {}

  dns_raw.map do |record|
    record = record.split(', ')
    if record_types.include?(record[0]) && record.length == 3
      dns_records[record[1]] = {
        :type => record[0],
        :destination => record[2][0..-3],
      }
    end
  end

  dns_records
end

# O(n) where n => number of alias of a domain
def resolve(dns_records, lookup_chain, domain)
  # pp dns_records
  record = dns_records[domain]
  if !record
    lookup_chain = ["NO RECORD FOUND"]
  elsif record[:type] == "CNAME"
    resolve(dns_records, lookup_chain.push(record[:destination]), record[:destination])
  elsif record[:type] == "A"
    lookup_chain.push(record[:destination])
  end
end


# To complete the assignment, implement `parse_dns` and `resolve`.
# Remember to implement them above this line since in Ruby
# you can invoke a function only after it is defined.
dns_records = parse_dns(dns_raw)
lookup_chain = [domain]
lookup_chain = resolve(dns_records, lookup_chain, domain)
puts lookup_chain.join(" => ")