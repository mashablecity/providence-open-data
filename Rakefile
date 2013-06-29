require 'rake'
require 'csv'
require 'json'

file "Providence_Tree_Inventory.geojson" => "Providence_Tree_Inventory.csv" do |t, args|
  trees = CSV.read(t.prerequisite_tasks[0].name, headers: true)

  features = trees.map{|tree|
    address = tree["Property Address"]

    if m = address.match(/^\((.*), (.*)\)$/)
      geometry = { type: "Point", coordinates: [m[2], m[1]] }
    end

    {
      type: "Feature",
      geometry: geometry,
      properties: tree.to_hash,
    }
  }

  feature_collection = {
    type: "FeatureCollection",
    features: features,
  }

  File.open(t.name, 'w') do |file|
    file << JSON.pretty_generate(feature_collection)
  end
end

task :default => "Providence_Tree_Inventory.geojson"
